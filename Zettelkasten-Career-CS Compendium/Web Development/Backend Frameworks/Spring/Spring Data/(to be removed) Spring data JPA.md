---
tags:
  - Spring
---
[toc]

-------------------------------------------------------------------------------
# The Spring Data Generated DAO – No More DAO Implementations

As we discussed in an earlier article, [the DAO layer](https://www.notion.so/Definitions-74f8db0ff42746a7bec70e166b97ed1c?pvs=21) usually consists of a lot of boilerplate code that can and should be simplified. The advantages of such a simplification are many: a decrease in the number of artifacts that we need to define and maintain, consistency of data access patterns, and consistency of configuration.

Spring Data takes this simplification one step further and **makes it possible to remove the DAO implementations entirely**. The interface of the DAO is now the only artifact that we need to explicitly define.

In order to start leveraging the Spring Data programming model with JPA, a DAO interface needs to extend the JPA specific _Repository_ interface, _JpaRepository_. This will enable Spring Data to find this interface and automatically create an implementation for it.

By extending the interface, we get the most relevant CRUD methods for standard data access available in a standard DAO.

# Custom Access Method and Queries

As discussed, **by implementing one of the _Repository_ interfaces, the DAO will already have some basic CRUD methods (and queries) defined and implemented**.

To define more specific access methods, Spring JPA supports quite a few options:

- simply **define a new method** in the interface
- provide the actual **JPQL query** by using the _@Query_ annotation
- use the more advanced **Specification and Querydsl support** in Spring Data
- define **custom queries** via JPA Named Queries

The [third option](http://spring.io/blog/2011/04/26/advanced-spring-data-jpa-specifications-and-querydsl/), Specifications and Querydsl support, is similar to JPA Criteria, but uses a more flexible and convenient API. This makes the whole operation much more readable and reusable. The advantages of this API will become more pronounced when dealing with a large number of fixed queries, as we could potentially express these more concisely through a smaller number of reusable blocks.

The last option has the disadvantage that it either involves XML or burdening the domain class with the queries.

## Automatic Custom Queries

When Spring Data creates a new _Repository_ implementation, it analyses all the methods defined by the interfaces and tries to **automatically generate queries from the method names**. While this has some limitations, it's a very powerful and elegant way of defining new custom access methods with very little effort.

Let's look at an example. If the entity has a _name_ field (and the Java Bean standard _getName_ and _setName_ methods), **we'll define the _findByName_ method in the DAO interface.** This will automatically generate the correct query:

```java
publicinterfaceIFooDAOextendsJpaRepository<Foo, Long> {

    FoofindByName(String name);

}Copy
```

This is a relatively simple example. The query creation mechanism supports [a much larger set of keywords](https://docs.spring.io/spring-data/data-jpa/docs/current/reference/html/#jpa.query-methods.query-creation).

In case the parser can't match the property with the domain object field, we'll see the following exception:

```java
java.lang.IllegalArgumentException: No property nam foundfor type class com.baeldung.spring.data.persistence.model.Foo
```

## Manual Custom Queries

Now let's look at a custom query that we'll define via the _@Query_ annotation:

```java
@Query("SELECT f FROM Foo f WHERE LOWER(f.name) = LOWER(:name)")
FooretrieveByName(@Param("name") String name);Copy
```

For even more fine-grained control over the creation of queries, such as using named parameters or modifying existing queries, [the reference](https://docs.spring.io/spring-data/data-jpa/docs/current/reference/html/#jpa.named-parameters) is a good place to start.

## Transaction Configuration

The actual implementation of the Spring-managed DAO is indeed hidden since we don't work with it directly. However, it's a simple enough implementation, **the _SimpleJpaRepository,_ which defines transaction semantics using annotations**.

More explicitly, this uses a read-only _@Transactional_ annotation at the class level, which is then overridden for the non-read-only methods. The rest of the transaction semantics are default, but these can be easily overridden manually per method.

## Exception Translation Is Alive and Well

The question now becomes: since Spring Data JPA doesn't depend on the old ORM templates (_JpaTemplate_, _HibernateTemplate_), and they have been removed since Spring 5, are we still going to get our JPA exceptions translated to Spring's _DataAccessException_ hierarchy?

The answer is, of course, we are. **Exception translation is still enabled by the use of the _@Repository_ annotation on the DAO**. This annotation enables a Spring bean postprocessor to advise all _@Repository_ beans with all the _PersistenceExceptionTranslator_ instances found in the container, and provide exception translation just as before.

Let's verify exception translation with an integration test:

```java
@Test(expected = DataIntegrityViolationException.class)
publicvoidwhenInvalidEntityIsCreated_thenDataException() {
    service.create(newFoo());
}Copy
```

Keep in mind that **exception translation is done through proxies.** In order for Spring to be able to create proxies around the DAO classes, these must not be declared _final_.
