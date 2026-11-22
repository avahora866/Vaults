---
tags:
  - FORMAT
  - Spring
---
# Understanding Pagination in Spring Data

Pagination is the process of breaking large datasets into smaller, manageable chunks to improve performance and user experience. Spring Data provides built-in support for pagination using the `Pageable` and `Page` interfaces.

## Offset Pagination (Standard Pagination)

* **How It Works:** Uses SQL’s `LIMIT` and `OFFSET` clauses.  
* **Example:**

```java
Page<User> findByRole(String role, Pageable pageable);
```

* **Pros:** Simple to implement and integrates seamlessly with Spring Data.  
* **Cons:** Performance degrades with high offsets because the database must scan and discard rows.

## Keyset Pagination (Seek Pagination)

* **How It Works:** Uses indexed columns (like primary keys or timestamps) to fetch the next set of results.  
* **Example:**

```java
@Query("SELECT u FROM User u WHERE u.id > :lastId ORDER BY u.id ASC")
List<User> findNextPage(@Param("lastId") Long lastId, Pageable pageable);
```

* **Pros:** Faster for large datasets as it avoids scanning skipped rows.  
* **Cons:** Requires an ordered, indexed column and cannot directly jump to arbitrary pages.

## Cursor-Based Pagination

* **How It Works:** Uses a cursor (a unique identifier) instead of an offset to fetch the next set.  
* **Example:**

```java
@Query("SELECT u FROM User u WHERE u.createdAt > :cursor ORDER BY u.createdAt ASC")
List<User> findNextPage(@Param("cursor") Instant cursor, Pageable pageable);
```

* **Pros:** Efficient and works well for real-time or infinite scrolling applications.  
* **Cons:** Not ideal for back-and-forth navigation.

# Handling Large Datasets Efficiently

* **Indexing:** Ensure indexed columns are used for pagination queries.  
* **Batch Processing:** Use `Slice<T>` instead of `Page<T>` to fetch only required data.  
* **Caching:** Store frequently accessed data to reduce database hits.  
* **Projection:** Fetch only required columns instead of entire entities to improve performance.

# Custom Sorting Logic in Spring Data

Spring Data allows sorting by multiple fields using `Sort` objects.

* **Example:** Sorting by name and age:

```java
Page<User> findAll(Pageable pageable);
```

* 

```java
Pageable pageable = PageRequest.of(0, 10, Sort.by("name").ascending().and(Sort.by("age").descending()));
```

## Custom Sorting Logic

For more complex sorting needs:

* **Derived Query Methods:** Use method names.

```java
List<User> findByOrderByLastNameAsc();
```

* **Custom Query with Sorting Parameter:**

```java
@Query("SELECT u FROM User u ORDER BY :sortField :sortDirection")
List<User> findUsersSorted(@Param("sortField") String sortField, @Param("sortDirection") String sortDirection);
```

# Best Practices

* Prefer **keyset pagination** for performance.  
* Use **database indexing** on sorted and paginated columns.  
* Avoid high `OFFSET` values in **large datasets**.  
* Implement **caching** for frequently accessed pages.  
* Optimize queries using **projection** to fetch only necessary fields.

Would you like me to refine this further or add implementation details?

