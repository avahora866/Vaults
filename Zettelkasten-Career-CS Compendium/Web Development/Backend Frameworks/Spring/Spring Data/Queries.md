---
tags:
  - Spring
  - FORMAT
---
There are several query languages used to interact with databases, particularly in Java-based applications. Here’s an overview of some key ones:

# HQL (Hibernate Query Language)

* **Purpose**: HQL is an object-oriented query language used with Hibernate, a Java ORM (Object-Relational Mapping) framework.  
* **Syntax**: Similar to SQL but operates on **entity objects** instead of database tables.  
* **Example**:

```java
String hql = "FROM Employee WHERE department = :dept";
Query query = session.createQuery(hql);
query.setParameter("dept", "IT");
List<Employee> results = query.list();
```

* **Key Features**:  
  * Supports relationships and inheritance.  
  * Can perform joins based on entity associations.

---

# JPQL (Java Persistence Query Language)

* **Purpose**: JPQL is part of JPA (Java Persistence API) and is similar to HQL but is standardized for JPA-based frameworks.  
* **Syntax**: Similar to HQL but follows **JPA specifications**.  
* **Example**:

```java
String jpql = "SELECT e FROM Employee e WHERE e.salary > :salary";
TypedQuery<Employee> query = entityManager.createQuery(jpql, Employee.class);
query.setParameter("salary", 50000);
List<Employee> results = query.getResultList();
```

* **Key Features**:  
  * Works with any JPA provider (e.g., Hibernate, EclipseLink).  
  * Can use entity relationships.

---

# SQL Criteria Query (JPA Criteria API & Hibernate Criteria API)

* **Purpose**: Used for **type-safe** and **dynamic** queries in Java, avoiding raw string-based queries.  
* **Types**:  
  * **JPA Criteria API** (introduced in JPA 2.0, replaces Hibernate Criteria API).  
  * **Hibernate Criteria API** (deprecated in favor of JPA Criteria API).  
* **Example (JPA Criteria API)**:

```java
CriteriaBuilder cb = entityManager.getCriteriaBuilder();
CriteriaQuery<Employee> cq = cb.createQuery(Employee.class);
Root<Employee> root = cq.from(Employee.class);
cq.select(root).where(cb.greaterThan(root.get("salary"), 50000));
TypedQuery<Employee> query = entityManager.createQuery(cq);
List<Employee> results = query.getResultList();
```

* **Key Features**:  
  * Type-safe queries using Java objects.  
  * Best for **dynamic query generation**.

---

Yes, besides **HQL**, **JPQL**, and **Criteria Queries**, there are a few more query options you should know about when working with Java-based persistence frameworks:

---

# Native SQL Queries

* **Purpose**: Allows direct execution of raw **SQL** against the database.

* **When to Use**:

  * When specific database features are needed (e.g., stored procedures, custom functions).  
  * When performance optimization is required.  
  * When complex queries are difficult to express in **HQL**, **JPQL**, or **Criteria API**.  
* **Example**:

```java
String sql = "SELECT * FROM employees WHERE department = :dept";
Query query = entityManager.createNativeQuery(sql, Employee.class);
query.setParameter("dept", "IT");
List<Employee> results = query.getResultList();
```

*   
  **Pros**:

  * Full control over SQL syntax.  
  * Can use database-specific features.  
* **Cons**:

  * Not portable (tied to a specific database dialect).  
  * Lacks type safety.  
  * Requires manual mapping if not returning entities directly.

---

# Spring Data JPA Query Methods

* **Purpose**: Allows **automatic query generation** from method names in **Spring Data JPA**.

* **When to Use**: For **simple queries** without needing manual JPQL or Criteria API.

* **Example**:

```java
public interface EmployeeRepository extends JpaRepository<Employee, Long> {
    List<Employee> findByDepartment(String department);
}
```

*   
  **Pros**:

  * Very concise.  
  * Type-safe.  
  * Integrates well with Spring ecosystem.  
* **Cons**:

  * Limited to simple queries.  
  * Can become cumbersome for complex queries.

---

# Query by Example (QBE)

* **Purpose**: Allows querying using **domain objects** as an example of the search criteria.

* **When to Use**: Useful for **dynamic queries** with minimal boilerplate code.

* **Example**:

```java
Employee exampleEmployee = new Employee();
exampleEmployee.setDepartment("IT");

ExampleMatcher matcher = ExampleMatcher.matching()
    .withIgnorePaths("id");

Example<Employee> example = Example.of(exampleEmployee, matcher);
List<Employee> results = employeeRepository.findAll(example);
```

*   
  **Pros**:

  * Simple to use for dynamic filters.  
  * No need for SQL or JPQL.  
* **Cons**:

  * Limited to equality-based matching.  
  * Not suitable for complex queries.

---

# Comparison Table of Query Approaches

| Query Type | Framework | Syntax | Use Cases | Pros | Cons |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **HQL** | Hibernate | Entity-based SQL | ORM-specific, entity-based queries | Supports relationships | Not type-safe |
| **JPQL** | JPA | Entity-based SQL | Portable across JPA providers | JPA standard compliance | Not type-safe |
| **Criteria API** | JPA, Hibernate | Java API | Type-safe, dynamic queries | Type-safe, dynamic | Verbose syntax |
| **Native Queries** | JPA, Hibernate, JDBC | Raw SQL | Complex or DB-specific queries | Full SQL control | Not portable, manual mapping |
| **Spring Data JPA** | Spring Framework | Method Names | Simple queries, repository pattern | Concise, type-safe | Limited query complexity |
| **Query by Example** | Spring Data JPA | Example objects | Dynamic queries with example objects | Easy to implement | Limited to equality matching |

---

# When to Use Native Queries

* **Performance Optimization**: When you need custom optimizations that aren't achievable with ORM-based queries.  
* **Database-Specific Features**: Leveraging stored procedures, triggers, window functions, or advanced SQL features.  
* **Complex Joins and Aggregations**: When queries are too complex for HQL/JPQL or Criteria API.  
* **Legacy Systems**: When integrating with existing databases where rewriting queries is not feasible.