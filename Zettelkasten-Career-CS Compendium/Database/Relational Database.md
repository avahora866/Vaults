---
tags:
  - "#TRUNK"
---

Relational databases are databases that store data in tables.
- A column (also called an attribute) defines a specific type of data (e.g., FirstName, EmailAddress, Price).
- A row (also called a tuple or record) represents a single item or entry (e.g., a specific customer, a single product).

# Relationships and joins
Relational tables can have relationships between tables:
- One to One
- One to Many
- Many to Many

# Properties of Relational Databases
## Transactions
Database transactions refer to the concept of two or more database operations being treated as a single operation

A transaction is a logical wrapper around a sequence of database operations that enforces an "all or nothing" rule. The system guarantees that either all operations within the transaction will complete successfully, or none of them will have any effect.

In SQL, this is initiated with a `BEGIN TRANSACTION` or `START TRANSACTION` command, followed by your operations, and then finalized with either:
- `COMMIT`: Make the changes permanent. Both updates are now visible to everyone.
- `ROLLBACK`: Undo all changes. The database is returned to the exact state it was in before the transaction began.

This "all or nothing" principle is what allows a complex application to remain consistent and correct, even in the face of errors or system failures.

## ACID
For a database system to be considered "transactional," it must guarantee these four properties. They are the "contract" between you and the database, ensuring your data remains sound despite concurrent use and system failures.
ACID is an acronym for:
- Atomicity
- Consistency
- Isolation
- Durability

### Atomicity
It is the "all or nothing" principle.
An "atom" is the smallest indivisible unit of matter. A transaction is the smallest indivisible unit of work. It either all happens, or it doesn't happen at all. The system will not allow a transaction to partially complete.

Example: The $100 transfer. Atomicity guarantees that you cannot only debit the savings account. The entire transaction (debit savings and credit checking) is the "atom." If the COMMIT succeeds, both actions happened. If it fails (or you ROLLBACK), it's as if neither ever occurred.

### Consistency
This is the most "high-level" concept. It guarantees that any transaction will bring the database from one valid state to another.

The database enforces rules to maintain validity. These are the constraints you define:
- NOT NULL (A required field must be filled)
- UNIQUE (No two customers can have the same email)
- FOREIGN KEY (You can't add a BookID to the Checkouts table if that book doesn't exist in the Books table)
- Business rules (e.g., CHECK (Balance >= 0)—a bank account balance can never be negative)

Consistency ensures that when a transaction COMMITs, all these rules are satisfied. If our transfer transaction accidentally made the savings balance -$10, a CHECK constraint would (or should) fire, causing the transaction to fail and roll back, preserving the database's consistent state.

### Isolation
This property addresses the problem of concurrency—many users and processes accessing the database at the same time.

Isolation guarantees that concurrently executing transactions will not interfere with each other. From the perspective of any single transaction, it appears as if it is the only transaction running on the database at that moment.

Analogy: 
Imagine two people trying to book the last airline seat (Seat 23A) at the exact same moment.
- Without Isolation: Both transactions read "1 seat available." Transaction 1 books it. Transaction 2 also books it. You now have two passengers for one seat. The data is corrupt.
- With Isolation: The database "isolates" them. It effectively "locks" the record. Transaction 1 reads "1 seat" and books it. Transaction 2 is forced to wait until Transaction 1 is finished. By the time Transaction 2 is allowed to read, Transaction 1 has COMMITted, and the value is now "0 seats available." Transaction 2 fails, as it should.

This prevents "race conditions" and ensures that transactions do not read partial, uncommitted, or "dirty" data from one another.

#### Isolation Levels
Isolation is, in practice, the most complex of the ACID properties because it is not a simple "on/off" switch. It's a spectrum.
Running all transactions serially (one after another) would provide perfect isolation, but in a high-traffic system, this would be unacceptably slow. The challenge is to allow transactions to run concurrently (at the same time) while preventing them from corrupting each other's work.

To manage this, the SQL standard defines four Isolation Levels. These levels represent a trade-off: each level provides more protection (consistency) at the cost of less concurrency (performance).

The levels are defined by the "phenomena," or read anomalies, they prevent. 
##### Level 1: READ UNCOMMITTED
This is the "loosest" and fastest isolation level. It essentially provides no isolation.
A transaction running at this level can read data that has been modified by other transactions, even if those transactions have not yet committed their changes.

###### The Anomaly It Allows: Dirty Read DIRTY
1. Transaction 1 (T1) UPDATEs a product's price from $20 to $100.
2. Transaction 2 (T2), running as READ UNCOMMITTED, SELECTs that product's price. It reads the "dirty" (uncommitted) value of $100.
3. T1 fails or decides to ROLLBACK its change. The price reverts to $20.

Result: T2 has made a business decision based on data ($100) that never officially existed in the database. This is highly dangerous, and this level is almost never used.

##### Level 2: READ COMMITTED
This is the default level for most databases, including PostgreSQL, as it offers a practical balance.
It solves the above Dirty Read problem. A transaction is only allowed to see data that has been formally COMMITTED.

How it works (in PostgreSQL): It uses Multi-Version Concurrency Control (MVCC). Your transaction is given a "snapshot" of the database at the moment your query begins. You will only see committed data from before that snapshot.

###### The Anomaly It Allows: Non-Repeatable Read (or "Fuzzy Read")
A Non-Repeatable Read occurs when you read the same row multiple times within a single transaction and get different values.

1. Your transaction (T1) SELECTs a product's price. It reads the committed value of $20.
2. Another transaction (T2) UPDATEs that product's price to $30 and COMMITs.
3. Later, within your same transaction (T1), you run the exact same SELECT query again.

Result: Because T2's change is now committed, your new query (which gets a new snapshot) now sees the $30 value. The same read, within one transaction, gave two different results. This is "fuzzy" and can break complex calculations.

##### Level 3: REPEATABLE READ
This level provides a much stronger guarantee for transaction-wide consistency.
It solves the above Non-Repeatable Read problem. It guarantees that any data you read will remain the same for the entire duration of your transaction

How it works (in MVCC): Your transaction is given a single "snapshot" of the database at the start of the transaction and uses this same snapshot for all queries. Even if T2 updates and commits a row you've read, you will continue to see the old version from your original snapshot.

###### The Anomaly It Allows: Phantom Read
A Phantom Read occurs when you run a query that selects a range of rows, and a subsequent, identical query within the same transaction finds new rows that have appeared

1. Your transaction (T1) `SELECT COUNT(*) FROM Employees WHERE Department = 'Sales'`. It finds 10 employees.
2. Another transaction (T2) INSERTs a new employee into the 'Sales' department and COMMITs.
3. Later, your transaction (T1) runs the exact same SELECT COUNT(*) query.

Result: It now finds 11 employees. A "phantom" row has appeared. REPEATABLE READ prevents changes to rows you've read, but it cannot lock rows that did not exist when your snapshot was taken.

##### Level 4: SERIALIZABLE
This is the "fortress" of isolation—the strongest and most "correct" level.

It solves the above Phantom Read problem. It guarantees that running transactions concurrently has the exact same outcome as if they were run one after another (serially). It prevents all anomalies.

How it works: This is highly complex. In PostgreSQL, it uses a technique called Serializable Snapshot Isolation (SSI). It optimistically allows transactions to run, but it monitors them for dependencies. If it detects that one transaction's outcome would have been different if it had run at a different time (i.e., a serialization anomaly is about to occur), it will automatically force one of the transactions to ROLLBACK with a "serialization failure" error.

Result: Perfect consistency, but at the highest performance cost. It introduces more overhead and can result in transactions failing and needing to be retried by your application logic.

### Durability
This is the simplest to understand: Once the database tells you a transaction has COMMITted, that change is permanent and will survive any subsequent system failure.
If you COMMIT the $100 transfer and the server immediately bursts into flames or the power cord is pulled, that transaction is not lost.

How: The system writes the changes to a "transaction log" (like a write-ahead log, or WAL, in PostgreSQL) before it reports success. When the server reboots, it will check this log and ensure any committed-but-not-yet-fully-written changes are correctly applied.

# Querying Relational Databases
To ask a relational database questions (to "query" it), we must use a specific, standardized language. This language is called SQL (Structured Query Language), pronounced either "S-Q-L" or "sequel."

SQL is a declarative language. This is a key concept.
- An imperative language (like C, Java, or Python) requires you to describe how to do something, step-by-step. ("Go to the first row, check its value, add it to a temporary variable...")
- A declarative language (like SQL) requires you to describe what you want. ("Give me the total sales for the 'Electronics' department.")

You tell the database what result you want, and the database's internal "query optimizer" is responsible for figuring out the most efficient how (e.g., whether to use an index, which table to read first, etc.).
All interactions—creating tables, inserting data, and especially retrieving data—are handled by a set of command keywords. The most important of all is SELECT.

## Core Keyword: SELECT
The SELECT statement is used only for retrieving data. It does not modify data in any way, making it a "read-only" operation.

Its most basic form consists of two parts:
- SELECT: Specifies the columns (attributes) you want to see.
- FROM: Specifies the table (relation) you want to retrieve them from.

The WHERE clause filters the rows (records) based on a condition, before any results are returned.

## JOIN (Combining Tables)
A JOIN clause is used with SELECT to combine rows from two or more tables based on a related column between them. This "related column" is almost always the Primary Key in one table (e.g., Students.StudentID) and the Foreign Key in the other (e.g., Checkouts.StudentID_FK).

There are several types of JOINs, but the most common by far is the INNER JOIN.

### INNER JOIN
An INNER JOIN retrieves only the rows that have a match in both tables. It is the "intersection" of the two sets of data.

### OUTER JOIN
An INNER JOIN is for finding the intersection—where both tables have a match.
But what about the opposite scenario? What if your question is, "Show me all my students, regardless of whether they have a book checked out or not"?
This is where OUTER JOINs are required. The most common and useful of these is the LEFT JOIN (or LEFT OUTER JOIN; the word OUTER is optional).

#### LEFT JOIN
A LEFT JOIN returns all rows from the "left" table (the table listed first, after FROM) and only the matching rows from the "right" table (the table listed after JOIN).

If a row in the left table has no match in the right table, the result will still include that row, but the columns from the right table will be filled with NULL (which means "unknown" or "missing data").

---

For completeness, there are two other OUTER JOINs:
- RIGHT JOIN: The exact opposite of a LEFT JOIN. It keeps all rows from the "right" table and fills NULLs for non-matches from the "left". (In practice, most engineers find it clearer to just rewrite the query as a LEFT JOIN by swapping the table order).
- FULL OUTER JOIN: Keeps all rows from both tables, matching where it can and filling with NULLs on both sides where it cannot.

The INNER JOIN and LEFT JOIN will handle over 95% of your needs.

## SET Operations
JOINs combine data horizontally (matching columns from different tables side-by-side). Set Operations, combine data vertically (stacking rows from one result set on top of another).

Set Operations are used to combine the results of two or more SELECT statements. For these to work, the SELECT statements must follow two strict rules:
- They must have the same number of columns.
- The columns must be of compatible data types (e.g., you can't stack a Name column on top of a Price column).

### UNION and UNION ALL
This is the most common set operation. It stacks the results of two queries into a single result set.
- UNION ALL: This is the simpler version. It takes all the rows from the first query and simply appends all the rows from the second query. It's fast and does no extra work.
- UNION: This does the same thing, but with one extra step: it removes any duplicate rows. Because it has to sort and filter the data to find duplicates, it is slightly slower than UNION ALL.

### INTERSECT
This operation returns only the rows that appear in both result sets. It is the "intersection" of the two queries, just like an INNER JOIN, but it compares entire rows, not just key columns.

### EXCEPT (or MINUS in some databases)
This operation returns only the rows that appear in the first result set but not in the second. It is a "subtraction" operation.

--- 
While JOINs are used constantly, Set Operations (UNION in particular) are powerful tools for specific cases, such as combining data from tables that have similar structures but represent different entities (e.g., CurrentEmployees and ArchivedEmployees).

# Normalisation
Normalisation is a formal, step-by-step process for organizing the columns and tables in a database to reduce data redundancy and prevent data integrity problems.
At its core, it's a set of rules to ensure that each piece of data is stored in only one place.

Imagine a lazy design where we just dump everything into one giant table, like a big spreadsheet:

| StudentID | StudentName | Major | BookID | BookTitle | DueDate | 
| :--- | :--- | :--- | :--- | :--- | :--- | 
| 102 | 'Bob' | 'History' | 77 | 'The Roman Empire' | 2025-11-10 | 
| 102 | 'Bob' | 'History' | 99 | 'World War II' | 2025-11-12 | 
| 101 | 'Alice' | 'Physics' | 88 | 'A Brief History of Time'| 2025-11-15 |

This table looks simple, but it's a minefield of problems caused by redundancy. Notice that 'Bob's' name and major are repeated for every single book he checks out. This redundancy leads to three specific types of problems, known as Data Anomalies:

1. Insertion Anomaly:
    - Problem: How do we add a new student, 'Charlie', who hasn't checked out a book yet?
    - Anomaly: We can't. The BookID and BookTitle would be NULL. The design forces us to have a book in order to have a student, which is illogical.
2. Update Anomaly:
    - Problem: Student 'Bob' changes his major from 'History' to 'Economics'.
    - Anomaly: We must find every single row where 'Bob' appears and update his major. If we miss even one row (e.g., we only update the 'World War II' record), the database is now in an inconsistent state. It claims Bob is in two different majors at the same time.
3. Deletion Anomaly:
    - Problem: Student 'Alice' returns her only book, 'A Brief History of Time', and we delete that checkout record.
    - Anomaly: In doing so, we have also deleted the only record that student 'Alice' exists. We have accidentally lost all information about her (her name, her major) simply because she returned her book.

The solution to all these problems is normalisation: We would split this one bad table into Students, Books, and Checkouts tables.

Normalisation is guided by a series of rules called Normal Forms (1NF, 2NF, 3NF, etc.). These are the specific steps you follow to eliminate these anomalies.

## First Normal Form (1NF): "The Key" (Atomicity)
Every cell in the table must contain a **single, atomic value**. You cannot have lists or arrays in a single column.

**Violation Example:** Imagine the `Checkouts` table had a column called `BooksBorrowed` that looked like this:

| **StudentID** | **Name** | **BooksBorrowed**                    |
| ------------- | -------- | ------------------------------------ |
| 102           | 'Bob'    | **"The Roman Empire, World War II"** |

This is a violation. The `BooksBorrowed` column contains two distinct pieces of information separated by a comma. You cannot easily query "Who borrowed 'World War II'?"
**The Fix:** You must break this into distinct rows so that each row represents _one_ fact.

|**StudentID**|**Name**|**BookBorrowed**|
|---|---|---|
|102|'Bob'|'The Roman Empire'|
|102|'Bob'|'World War II'|
Now the table is in **1NF**.
## Second Normal Form (2NF): "The Whole Key"
**Rule:** The table must be in 1NF, and all non-key columns must depend on the **entire** Primary Key, not just part of it.

_Note: This only applies if you have a **Composite Primary Key** (a key made of two or more columns)._

**Violation Example:** Let's look at that "Bad_Library_Data" table again. Assume the Primary Key is the combination of **(StudentID + BookID)** because a student can check out many books, and a book can be checked out by many students (over time).

|**StudentID**|**BookID**|**StudentName**|**BookTitle**|
|---|---|---|---|
|**102**|**77**|'Bob'|'The Roman Empire'|
- **Key:** (StudentID, BookID)
- **Column:** `StudentName` ('Bob')

**The Problem:** Does 'Bob' depend on the _book_? No. 'Bob' is determined _solely_ by `StudentID` (102). He is 'Bob' regardless of which book he borrows. This is a **Partial Dependency**—`StudentName` depends on only _part_ of the primary key.

**The Fix:** Split the data. Move `StudentName` to a separate `Students` table where `StudentID` is the _full_ key. Move `BookTitle` to a `Books` table.

Now the tables are in **2NF**.

## Third Normal Form (3NF): "Nothing But the Key"
**Rule:** The table must be in 2NF, and there must be no **Transitive Dependencies**.

**Violation Example:** Imagine our `Students` table looked like this:

|**StudentID**|**Name**|**ZipCode**|**City**|
|---|---|---|---|
|102|'Bob'|90210|'Beverly Hills'|
- **Key:** `StudentID`
- **Dependency:** `City` depends on `ZipCode`. (If you know the Zip, you know the City).
- **Transitive Chain:** `StudentID` -> `ZipCode` -> `City`.

**The Problem:** `City` is a fact about the `ZipCode`, not a direct fact about the `Student`. If the Post Office re-zones 90210 to be a different city, you would have to update every single student record living in that zip code.

**The Fix:** Create a `ZipCodes` table (`ZipCode`, `City`) and remove `City` from the `Students` table. The `Students` table simply keeps the `ZipCode`.

Now the database is in **3NF**.

---
1. **1NF**: No lists in cells (Atomic).
2. **2NF**: Don't mix data about the "subject" (Student) with data about the "action" (Checkout).
3. **3NF**: Don't store data that can be derived from other data columns (like City from Zip).

Most production databases aim for 3NF. Going further (4NF, 5NF) is usually only necessary for very specific academic or complex data warehousing scenarios.

# Indexing
Imagine you are holding a 1,000-page textbook on Biology, and I ask you to find the section on "Photosynthesis." 
You have two options:

1. Full Table Scan: You start at page 1, read every word on every page until you find "Photosynthesis".
    - Time Complexity: $O(n)$ (Linear time). If the book is twice as long, it takes twice as long. This is incredibly slow.
2. Index Lookup: You flip to the Index at the back of the book, find "Photosynthesis," read the page number (e.g., Page 42), and flip directly to that page.
    - Time Complexity: $O(\log n)$ (Logarithmic time). This is incredibly fast, even for massive books.

In a database, an Index is exactly that: **a separate data structure that stores the value of a specific column (like StudentName) and a pointer (address)** to the exact row where that data lives on the hard drive.

Most database indexes (in PostgreSQL, MySQL, Oracle) default to using a B-Tree (Balanced Tree).

A B-Tree is a sorted, self-balancing tree structure.

- The Root node contains ranges of values (e.g., A-M, N-Z).
- It points to Leaf nodes, which eventually contain the specific key and the pointer to the data.

When you run `SELECT * FROM Students WHERE Name = 'Alice'`, the database doesn't scan the whole table. It traverses the B-Tree:
- Is 'Alice' in the A-M branch? Yes. Go left.
- Is 'Alice' in the A-G branch? Yes. Go left.
- Found 'Alice'. Here is the pointer to her row.

This reduces the work from millions of operations to just 3 or 4 "hops."

## Trade-Off
Every time you create an index, you incur a Write Penalty.

When you INSERT a new row, UPDATE a value, or DELETE a row, the database must:
1. Update the actual table data.
2. Update every single index associated with that table (re-balancing the B-Tree).

If you have 10 indexes on a table, a single INSERT becomes 11 write operations.
- Heavy Read / Low Write System (e.g., Reporting): Use many indexes.
- Heavy Write System (e.g., IoT Log Ingestion): Use very few indexes.

## Clustured vs Non-Clustered
### 1. Clustered Index ( The "Dictionary")
- A Clustered Index determines the physical order of the data on the disk.
- Think of a dictionary: the words are the data, and they are sorted alphabetically. You don't look up "Apple" in an index at the back; you just go to the "A" section.
- Constraint: Since data can only be sorted physically in one way, a table can have only one Clustered Index.
- Default: In most databases, the Primary Key is automatically the Clustered Index.

### Non-Clustered Index (The "Textbook Index")
- This is a separate structure stored apart from the table data.
- It contains the value (e.g., Major = 'Physics') and a pointer to the row in the Clustered Index.
- You can have many Non-Clustered Indexes on a single table.

---
- Primary Keys get a free, fast Clustered Index.
- Foreign Keys usually need a manual Non-Clustered Index (because we often JOIN on them).
- Any column used frequently in a WHERE or ORDER BY clause is a candidate for an index.
- Always weigh the read-speed benefit against the write-speed penalty.

[[B-Tree]] #TODO include walkthrough of searching for alice in gemni conversation inside linked note https://gemini.google.com/share/a6ae26fd458c


# Server-Side Logic
Relational databases allow you to push some business logic directly into the database engine.

1. Performance: It reduces network traffic. Instead of sending 10 different SQL commands back and forth over the network, you send one command ("Run Procedure X") and the database does the heavy lifting locally.
2. Consistency: If multiple applications (e.g., a Web App and a Mobile App) use the same database, defining the logic in the database ensures both apps behave exactly the same way.

## Stored Procedures
A Stored Procedure is a pre-compiled collection of SQL statements stored under a name. It can accept parameters (inputs) and can perform complex actions like modifying data, creating tables, or running transactions.
Example: Imagine a procedure called RegisterNewStudent.
1. Check if 'John Doe' already exists.
2. If not, INSERT into Students.
3. Log the timestamp in an AuditLog table.
4. Return the new StudentID.

You can call stored procedure as so:
```sql
CALL RegisterNewStudent('John Doe', 'Math');
```

## User-Defined Functions (UDFs)
Functions are similar to procedures, but with a strict difference: Functions must return a value, and they are designed to be used inside a standard SQL query (like in a SELECT statement).
Example: A function CalculateLateFee(DueDate).
- compares the DueDate to the current date and multiplies the days overdue by $0.50.
```sql
SELECT
    BookTitle,
    CalculateLateFee(DueDate) AS OwedAmount
FROM Checkouts;
```
Note: Stored Procedures generally cannot be used inside a SELECT statement like this.

## Triggers
A Trigger is a piece of code that automatically "fires" (executes) in response to a specific event on a specific table. The events are usually INSERT, UPDATE, or DELETE.
Example: The Audit Trail You want to track every time a student's major is changed.
- Create Trigger: AfterUpdateMajor on the Students table.
- Logic: Whenever an UPDATE occurs on the Major column, take the old value and the new value and INSERT them into a MajorHistory table alongside the current timestamp.

Now, if you run:
```sql
UPDATE Students SET Major = 'History' WHERE ID = 102;
```
The database updates the record AND silently runs your trigger code in the background to update the history table.

## Logic in App vs. Logic in DB
Pros of DB Logic,Cons of DB Logic
Performance: Code runs right next to the data.,Hard to Debug: DB tools usually have poor debuggers compared to IDEs like VS Code.
Security: You can grant permission to run a procedure without giving permission to read the raw tables.,"Vendor Lock-in: SQL logic (PL/pgSQL for Postgres, T-SQL for SQL Server) is often proprietary. Moving to a different database later is very hard."
"DRY Principle: Write once, use by all apps.",Version Control: It is historically harder to version control DB code than file-based application code.

# Database Security
## The Threat: SQL Injection (SQLi)
This is the single most common and dangerous vulnerability in database-driven applications. It occurs when you construct SQL queries by simply concatenating strings together.

The Vulnerable Code: Imagine a login screen where you take a username from a text box and insert it directly into a query string in your code (e.g., Python/Java).

```sql
-- What you intend to run:
SELECT * FROM Users WHERE username = 'Alice';

-- How you wrote the code (Pseudocode):
query = "SELECT * FROM Users WHERE username = '" + userInput + "'";
```
The Attack: A hacker enters the following into the username box: Alice' OR '1'='1
The Resulting Query: The database receives this:
```sql
SELECT * FROM Users WHERE username = 'Alice' OR '1'='1';
```
- '1'='1' is always TRUE.
- Therefore, the WHERE clause is always TRUE for every single row.
- The database returns the entire Users table, effectively bypassing the login check or dumping all user data.

The Solution: Prepared Statements (Parameterized Queries) Never concatenate user input. Instead, use Prepared Statements.
You send the SQL template to the database first, with placeholders (? or $1).
```sql
SELECT * FROM Users WHERE username = $1;
```
Then, you send the user input (Alice' OR '1'='1) as a separate data packet. The database treats it strictly as a literal string value, not executable code. It will look for a user whose literal name is "Alice' OR '1'='1", find nothing, and remain safe.

## Access Control: The Principle of Least Privilege
Even if your code is perfect, you should not connect to the database as the "root" or "admin" user (like the postgres superuser).
If an attacker does find a vulnerability, you want to limit the damage they can do.

The Strategy:
1. Create Users (Roles): Create a specific user for your application (e.g., app_user).
2. GRANT Permissions: Explicitly give that user only the permissions they need.
    - Does the web app need to drop tables? No.
    - Does it need to change the schema? No.
    - Does it need to read the Salaries table? Maybe not.

```sql
-- Create a restricted user
CREATE USER app_user WITH PASSWORD 'secure_pass';

-- Give them read/write access to specific tables only
GRANT SELECT, INSERT, UPDATE ON Students TO app_user;

-- DO NOT give them DELETE or DROP permissions if they don't need them!
```
This is the Principle of Least Privilege: Give a user the absolute minimum access required to do their job, and nothing more.

## Encryption (Brief Overview)
1. Encryption in Transit: Just like HTTPS for the web, your application should connect to the database using SSL/TLS. This prevents anyone on the network (like a public Wi-Fi hacker) from "sniffing" the packets and reading the data as it travels between your app and the database server.
2. Encryption at Rest: This ensures that if someone physically steals the hard drive out of the server, they cannot read the files. This is usually handled by the database software itself (e.g., TDE - Transparent Data Encryption) or the file system.
