---
tags:
  - "#LEAF"
---
# Introduction
#TODO

# Abstract Query Languages (HQL, JPQL)
These languages allow you to write queries that look like SQL, but instead of targeting tables and columns, they target Classes and Properties. The ORM engine then translates your code into actual SQL to run on the database.

## JPQL (Java Persistence Query Language)
JPQL is the industry standard (part of the Jakarta EE / JPA specification). It is "database agnostic." You write JPQL once, and if you switch your database from PostgreSQL to Oracle, you don't have to rewrite your queries. The underlying engine handles the translation.

## HQL (Hibernate Query Language)
Hibernate is the most popular ORM framework for Java. HQL was the predecessor to JPQL. It is very similar but offers a few more advanced features that are specific to Hibernate and not part of the standard JPA spec.

In modern engineering, JPQL and HQL are often used interchangeably, but JPQL is preferred for portability.

---

The "Implicit Join" Magic This is the biggest benefit. In SQL, if you want to find a student's book title, you must write the JOIN.
```sql
SELECT b.title
FROM Students s
JOIN Checkouts c ON s.id = c.student_id
JOIN Books b ON c.book_id = b.id
WHERE s.name = 'Alice';
```

JPQL: Because your Java classes already have relationships defined (e.g., student.getCheckouts()), you can often navigate the relationship using dots, just like code.

```jpql
SELECT c.book.title
FROM Student s
JOIN s.checkouts c
WHERE s.name = 'Alice'
```

The ORM figures out the Foreign Keys and Join logic for you.
Other Ecosystems: While HQL/JPQL are specific to Java, this concept exists everywhere:
- C# / .NET: Uses LINQ (Language Integrated Query). You write actual C# code (db.Students.Where(s => s.Name == "Alice")), and it compiles to SQL.
- Python (Django/SQLAlchemy): You rarely write a query string at all. You use methods: Student.objects.filter(name='Alice').

## History of HQL and JPQL
1. HQL came first.
    - Hibernate was created by Gavin King in 2001. He invented HQL (Hibernate Query Language) to allow developers to query objects easily.
2. JPA came later.
    - The Java community saw how popular Hibernate was and decided to create a standard specification so everyone could do this. This became JPA (Java Persistence API).
3. JPQL was born from HQL.
    - When designing the query language for the standard (JPA), the committee largely copied HQL but stripped out the Hibernate-specific features to make it "pure." They called this subset JPQL.