---
tags:
  - "#LEAF"
---
When you send a SQL query to the database, it doesn't just run it. It passes it to a component called the Query Optimizer. The Optimizer treats your query like a math problem: it calculates multiple different ways ("plans") to retrieve the data and statistically estimates which path will be the "cheapest" (fastest/least CPU intensive).

To see this path, we use the keyword EXPLAIN (or EXPLAIN ANALYZE in PostgreSQL).
Analogy: The GPS If you type a destination into Google Maps, it calculates a route. It might say: "Take Highway 1 (Fastest)" or "Take Backroads (Shortest Distance)". The Explain Plan is that route. It tells you exactly which tables it will visit, in what order, and which indexes it will use.

# Reading a Plan: Scan Types
The most important thing to look for in an Explain Plan is the Scan Type. This tells you how the database is retrieving the rows.

## Sequential Scan (Seq Scan) / Full Table Scan
- What it is: The database reads every single row in the table, from top to bottom.
- The GPS Equivalent: Driving down every single street in the city to find one specific house.
- Verdict: Bad for large tables. If you see this on a table with millions of rows, you are likely missing an index.

## Index Scan
- What it is: The database uses the B-Tree index to find the specific row IDs, then grabs just those rows.
- The GPS Equivalent: Looking up the address in a directory and driving straight there.
- Verdict: Good. This is usually what we want.

## Index Only Scan
- What it is: The database finds the answer inside the index itself and doesn't even need to look at the main table (Heap).
- Example: SELECT Name FROM Students WHERE Name = 'Alice'. If you have an index on Name, the database reads the index, finds 'Alice', and stops. It never touches the heavy table data.
- Verdict: Excellent. This is the fastest possible operation.

## Example Scenario
You have a Users table with 1 million rows. You run:
```sql
EXPLAIN SELECT * FROM Users WHERE email = 'test@example.com';
```
- Outcome A (Bad): -> Seq Scan on users (cost=0.00..18345.00 rows=1 ...)
    - Diagnosis: The database had to read all 1 million rows because there is no index on email.
- Outcome B (Good): -> Index Scan using users_email_idx on users (cost=0.00..8.27 rows=1 ...)
    - Diagnosis: The database used an index (users_email_idx) and jumped straight to the record. The "cost" dropped from 18,345 to 8.27.

# SARGability
SARGable stands for Search ARGument able.
It asks a simple question: "Is your query written in a way that allows the database to use the index?"
You might have the perfect index on a column, but if you write your query poorly, the database will ignore the index and revert to a slow Sequential Scan.

The Golden Rule of SARGability is: Never wrap the column in a function or calculation.

You must perform calculations on the input value (the right side), not the column itself (the left side).

The three most common violations

## Functions on Columns
The Scenario: You want to find all users born in 1990. You have an index on the BirthDate column.
The Non-SARGable Query (Bad):
```sql
-- "Extract the year from the column, then compare it."
SELECT * FROM Users
WHERE YEAR(BirthDate) = 1990;
```

Why it fails: Imagine a phone book arranged by full date. If I ask you, "Find everyone born in 1990," you can't just jump to a section. You have to look at every single entry, calculate the year in your head, and check if it matches. The database must do the same. It runs the YEAR() function on every single row, rendering the index useless.

The SARGable Query (Good):
```sql
-- "Find dates within the range of that year."
SELECT * FROM Users
WHERE BirthDate >= '1990-01-01'
  AND BirthDate < '1991-01-01';
```

Why it works: We left the BirthDate column alone. We compared it to a pure range. The database can now jump straight to the start of 1990 in the B-Tree and stop reading as soon as it hits 1991.

## The Leading Wildcard
The Scenario: You are searching for a user named "Alice".

The Non-SARGable Query (Bad):
```sql
SELECT * FROM Users
WHERE Name LIKE '%Alice';
```

Why it fails: The % at the start means "anything can be here." In a sorted dictionary (B-Tree), "Alice" is under A. But "%Alice" could be "Malice" (under M) or "Chalice" (under C). The database has no idea where to look, so it must read everything (Full Table Scan).

The SARGable Query (Good):
```sql
SELECT * FROM Users
WHERE Name LIKE 'Alice%';
```

Why it works: The wild card is at the end. The database knows the word starts with 'A', so it can use the index to jump to the 'A' section.

## Implicit Type Casting
The Scenario: Your PhoneNumber column is stored as a string (VARCHAR), but the user inputs a number.

The Non-SARGable Query (Bad):
```sql
-- Input is a number (integer), Column is a String
SELECT * FROM Users WHERE PhoneNumber = 5551234;
```
Why it fails: The database sees a type mismatch. It says, "I can't compare a String to an Integer. I must convert the column to an Integer first." It silently rewrites your query to: WHERE CAST(PhoneNumber AS INT) = 5551234. Because it cast the column, the index is ignored.

The SARGable Query (Good):
```sql
-- Input is a String
SELECT * FROM Users WHERE PhoneNumber = '5551234';
```
Summary for Engineers: To keep a query SARGable, imagine the column is "locked" inside a glass box. You cannot touch it, multiply it, format it, or cast it. You must do all the work on the other side of the equals sign to match the column's format.

# Common Anti-Patterns
These are architectural mistakes—often made by the application code—that flood the database with unnecessary work.

## The Glutton: SELECT *
We discussed SELECT earlier, but we didn't discuss the danger of the asterisk (*).

The Problem: When you write SELECT *, you are asking for every single column the table has.
1. Network I/O: If your table has a huge text column (like Biography or ImageBlob) that you don't actually need for your current page, you are dragging megabytes of useless data across the network wire.
2. Killing the "Index Only Scan": Remember the "Index Only Scan"? It only works if the index contains all the columns you asked for. If you ask for *, the index definitely doesn't have everything, so the database is forced to look up the main table (Heap) for every single row.

The Fix: Always specify exactly the columns you need.
- Bad: SELECT * FROM Students
- Good: SELECT Name, Major FROM Students

## The Loop of Death: The "N+1 Select" Problem
This is the most common performance killer for software engineers using ORMs (like Hibernate/JPA or Django).
The Scenario: You want to display a list of 100 students and the name of their major.

The Naive Code (Pseudocode):
```python
# 1. Get all students (1 Query)
students = db.query("SELECT * FROM Students")

# 2. Loop through them to print details
for student in students:
    # 3. Access the major (Triggers a NEW query for every student!)
    print(student.name, student.major.name)
```
What happens underneath:
1. The database runs 1 query to get the list of 100 students.
2. The loop runs 100 times. Inside each iteration, the ORM detects you need the Major name, so it fires a separate query to the Majors table: SELECT * FROM Majors WHERE ID = ?.
Total Queries: 1 (list) + 100 (details) = 101 Queries. If you have 10,000 students, you just bombarded your database with 10,001 queries to load a single web page. The latency will be enormous.

The Fix: Eager Loading (JOIN) You must tell the database to fetch everything in one shot using a JOIN.
- SQL: `SELECT s.Name, m.MajorName FROM Students s JOIN Majors m ON ...`
- ORM (JPA): `SELECT s FROM Student s JOIN FETCH s.major`
This reduces 101 queries down to 1 query.