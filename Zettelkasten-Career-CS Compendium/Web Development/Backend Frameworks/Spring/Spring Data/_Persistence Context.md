---
tags:
  - Spring
---
  
**The persistence context is the first-level cache where all the entities are fetched from the database or saved to the database.** It sits between our application and the database (persistent storage).

Persistence context keeps track of any changes made into a managed entity. If anything changes during a transaction, then the entity is marked as dirty. When the transaction completes, these changes are flushed into persistent storage.

# Entity Manager

The `EntityManager` is the interface that lets us interact with the persistence context. **Whenever we use the `EntityManager`, we are actually interacting with the persistence context.**

## Hibernate’s Session

Session is the implementation of entity manager developed by hibernate to interact with the persistence context. It is what is used under the hood if you use `EntityManger` and your application is configured to use Hibernate as its JPA vendor.

# Persistence Context types
* Transaction-scoped persistence context  
* Extended-scoped persistence context

## Transaction-scoped persistence context

The transaction persistence context is bound to the transaction. As soon as the transaction finishes, the entities present in the persistence context will be flushed into persistent storage.

When we perform any operation inside the transaction, the `EntityManager` checks for a persistence context. If one exists, then it will be used. Otherwise, it will create a persistence context.

The default persistence context type is `PersistenceContextType.TRANSACTION`. To tell the `EntityManager` to use the transaction persistence context, we simply annotate it with `@PersistenceContext`:
```java
@PersistenceContext
private EntityManager entityManager;
```

Once the transaction has been completed and closed so too will this type of persistence context. As there is no need for an open persistence context without a transaction. **Persistence context is bound to the transaction that opened it.**

## Extended-scoped persistence context

An Extended-Scoped Persistence Context in JPA is a type of persistence context that maintains its state beyond a single transaction. This is useful in scenarios where you need to work with entities over multiple transactions or across different method calls within a business process.

* An extended persistence context remains active for a longer duration than the standard transaction-scoped persistence context. It can be active for the entire lifespan of a conversation or a user session.  
* Extended persistence contexts are typically used in applications that require a long-running context, such as web applications or applications that follow a conversation pattern (like JSF).

**The Extended-scoped persistence context is usually open for longer and can probably be said to be bound to the lifecycle of the application as it is NOT bound by each individual transaction.**

You can also manually close down the persistence context or set a timeout.

Some applications use a conversation scope which can close the persistence context depending on user actions

## Flushing

Flushing is the process of moving the changes from the persistence context to its database (persistent storage). It usually occurs when the transaction is closed, however there are other times when the persistence context is flushed as well:

- Manual call to flush  
- Automatic Flush  
  - Query Execution: Before executing a query JPA will flush the persistence context to make sure it gets the most up to date data  
  - Timeout: You can configure the persistence context to flush automatically at repeating times

## Concerns

### What if we have multiple entity managers in my application with different persistence context types

This is very possible however, if you have a transaction scoped persistence context whos transaction modified an entity that is also managed by the extended persistence context. The extended persistence context will need to be refreshed especially if the entity was loaded into the extended persistence context beforehand