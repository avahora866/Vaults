# The Repository Abstraction & Proxy Mechanism
The Theory: In traditional Data Access Object (DAO) patterns, you write a class, implement an interface, and inject the EntityManager. Spring Data inverts this. You define the Interface, and Spring generates the Implementation at runtime.

The Mechanism: When you start your application, Spring scans your classpath for interfaces extending Repository. For each one found, it uses JDK Dynamic Proxies to create a proxy instance that intercepts all method calls.

Think of the Proxy as a generic "gatekeeper." When you call findByUsername("john"), the Proxy intercepts the call and asks a QueryExecutor: "I have a method named 'findByUsername' and the argument 'john'. Please generate the SQL and execute it."

The Hierarchy: We have a strict hierarchy of interfaces. You must choose the one that fits your exposure level:
1. Repository<T, ID>: The "Marker" interface. It has zero methods. Use this if you want strict control and want to expose only the methods you define manually.
2. CrudRepository<T, ID>: Adds standard CRUD (save, findById, delete, count).
3. PagingAndSortingRepository<T, ID>: Adds findAll(Pageable) and findAll(Sort).
4. JpaRepository<T, ID> (Store specific): Extends everything above plus JPA-specific logic like flush() and saveAndFlush().

Analogy:
- Legacy DAO: You are a chef cooking every meal (writing every query) by hand.
- Spring Data Repository: You are the Head Chef (Interface). You shout orders ("Find me the user!"), and a brigade of line cooks (The Proxy) instantly assembles the dish based on the name of your order.

# Query Derivation Mechanics
Now that we have an interface, let's discuss how the Proxy understands your intent. How does Spring translate a Java method name into a SQL query?

It uses a parser based on a Subject-Predicate grammar, often referred to internally as the PartTree.

## The Grammar Structure
When the application context loads, Spring Data parses every method in your repository interface. It breaks the method name into two main parts:
- The Subject (Introductory Clause): This tells Spring what you want to do.
    - Examples: find...By, read...By, query...By, count...By, delete...By.
    - Note: The text between find and By is generally ignored unless it contains specific keywords like Distinct. So, findUserBy... and findBy... are functionally identical.
- The Predicate (Criteria): This follows the By keyword. It represents the WHERE clause of your SQL.
    - It is a chain of Properties connected by Operators (And, Or).

## The Internal Parser (The PartTree)
Let's look at a concrete example:
`List<User> findByEmailAddressAndLastname(String email, String lastname);`

Spring breaks this down into "Parts":
1. Strip the Subject: Removes findBy.
2. Split by Operator: Detects And.
    - Part 1: EmailAddress
    - Part 2: Lastname
3. Property Resolution: It looks at your User entity.
    - Does User have a lastname field? Yes. -> SQL: last_name = ?
    - Does User have an emailAddress field? Or perhaps an email field with an address sub-field?

## Property Traversal (The Ambiguity Problem)
This is where it gets complex. If your Entity graph looks like this:
```java
class User {
    private Address address;
    private String addressZipCode; // Flat field
}

class Address {
    private String zipCode; // Nested field
}
```
And you write: findByAddressZipCode

Spring's algorithm is greedy. It reads left-to-right:
1. Does User have AddressZipCode? (No)
2. Does User have AddressZip and that has Code? (No)
3. Does User have Address and that has ZipCode? (Yes!)

It assumes you mean the nested Address.zipCode.

However, what if you actually wanted to query the flat string addressZipCode on the User class? Spring's guess would be wrong, and your application might fail at startup or query the wrong column.

To resolve ambiguity in Derived Queries, you use the underscore character (_) to manually mark the "split point" for nested properties.
- findByAddress_ZipCode: This tells Spring, "Stop at Address, traverse into that object, and look for ZipCode."
- findByAddressZipCode: This tells Spring to prioritize finding a direct property named addressZipCode on the User class itself.

# Manual Query Overrides & SpEL
We have now reached the limit of what findBy... method names can reasonably do. When your queries become complex, involve joins, or require tuning, you must override the automatic generation.

We do this using the @Query annotation.

## The Basics: JPQL vs. Native
You can define queries in two languages directly above your method:
JPQL (Java Persistence Query Language): This queries the Entity Object, not the database table. It is database-agnostic.
```java
@Query("select u from User u where u.emailAddress = ?1")
User findByEmail(String email);
```

Native SQL: This queries the Database Table. It is tightly coupled to your specific DB (Postgres, Oracle, etc.).
```java
@Query(value = "SELECT * FROM users u WHERE u.email_address = ?1", nativeQuery = true)
User findByEmailNative(String email);
```

## The Advanced Mechanic: SpEL (Spring Expression Language)
This is the "Advanced" concept for this module.

Imagine you created a generic BaseRepository<T, ID> that you want all your entities to extend. You want a method that finds entities where a standard field isDeleted = false.

If you write: @Query("select e from Entity e where e.isDeleted = false")... This will fail. Why? Because JPQL needs to know the specific class name (e.g., from User, from Order). It cannot handle a generic "Entity".

The Solution: Spring Data exposes a SpEL variable #{#entityName} that resolves to the entity type of the repository at runtime.

Example:
```java
public interface BaseRepository<T, ID> extends JpaRepository<T, ID> {

    @Query("select e from #{#entityName} e where e.isDeleted = false")
    List<T> findAllActive();
}
```
When you create UserRepository extends BaseRepository<User, Long>, Spring dynamically rewrites #{#entityName} to User.

# Projections & Result Mapping
Until now, we have been fetching entire Entities (e.g., User).

The Problem: If your User entity has 50 fields, including large BLOBs or complex relationships, but you only need to display the user's "Full Name" on a dashboard, fetching the whole object is wasteful (memory and I/O).

The Solution: Projections. This allows you to fetch a specific "view" of the data.

Spring Data offers two powerful ways to do this without writing manual DTO converters.

## Interface-based Projections (The "Dynamic Proxy" approach)
You simply define an interface with getters matching the properties you want.
```java
// The Projection
public interface NamesOnly {
    String getFirstname();
    String getLastname();
}

// The Repository
public interface UserRepository extends Repository<User, Long> {
    List<NamesOnly> findByLastname(String lastname);
}
```

What happens internally? Spring Data creates a Proxy of NamesOnly on the fly.
- Closed Projections: If the getter matches a field exactly (getFirstname -> firstname), Spring optimizes the query to select only that column.
  - SQL generated: SELECT firstname, lastname FROM user ...
- Open Projections: You can use SpEL (@Value) to compute new values.
  - @Value("#{target.firstname + ' ' + target.lastname}") String getFullName();
  - SQL generated: SELECT * FROM user ... (Why? Because Spring cannot guess which fields are needed for the SpEL expression, so it must fetch the whole entity first).
  
## Class-based Projections (The DTO approach)\
If you prefer concrete classes (perhaps to ensure immutability or for serialization), you can use a Java record or a POJO.
```java
// The DTO
public record UserSummary(String firstname, String email) {}

// The Repository
List<UserSummary> findByEmail(String email);
```
The Mechanic: Spring determines this is a DTO projection by looking at the return type. It selects the matching fields from the database and invokes the constructor of your DTO. This is highly optimized.

# Dynamic Query Composition
Everything we have discussed so far (Method Names, @Query) is static. You write the query at compile time.

But what if you have a "Search Users" screen with 10 optional filters (Name, Date Range, Status, Zip Code, etc.)?
- If you use method names, you would need findByNameAndStatus, findByName, findByStatus, etc. That is an exponential explosion of methods.
- We need Dynamic Queries.

Spring Data provides two primary engines for this: Query by Example (QBE) and Specifications.

## Query by Example (QBE)
This is the "Simple but Limited" approach. You create an instance of your entity, populate the fields you want to search for, and tell Spring "Find entries that look like this."
```java
User userProbe = new User();
userProbe.setActive(true);
userProbe.setLastname("Smith");

Example<User> example = Example.of(userProbe);
repo.findAll(example);
```

- Pros: Very readable, no extra code.
- Cons: No support for complicated logic (like OR between different fields, ranges > <, or nested constraints). It is strictly "Field A = X AND Field B = Y".

## Specifications (The Powerhouse)
This is a wrapper around the JPA Criteria API. It allows you to build WHERE clauses programmatically.

The core interface is:
```java
public interface Specification<T> {
    Predicate toPredicate(Root<T> root, CriteriaQuery<?> query, CriteriaBuilder builder);
}
```
You can chain these together:
```java
repo.findAll(hasLastname("Smith").and(isActive().or(isNewUser())));
```
The Mechanism: When you execute findAll(spec), Spring takes the Root (the table), the CriteriaBuilder (the factory), and runs your toPredicate logic to assemble the final SQL WHERE clause dynamically before sending it to the database.

# Custom Repository Fragments
This is a problem every senior developer faces eventually: "The Magic Wall."

You have a UserRepository doing standard CRUD. Suddenly, you need a method complexReport() that requires:
- Raw JDBC for performance.
- Or a call to an external REST API.
- Or manual handling of the EntityManager.

You cannot put this code inside the interface (interfaces can't hold stateful logic). You cannot modify the Proxy class Spring generates.

The Solution: The Fragment Pattern (Composition). Spring Data allows you to "plug in" your own code into the auto-generated proxy.

## The Implementation Steps
1. Define a Custom Interface (The Fragment): This defines only the custom method(s).
```java
public interface UserRepoCustom {
    void complexReport();
}
```
2. Implement the Fragment: Write a standard Java class implementing that interface.
   - Crucial Naming Convention: By default, Spring looks for a class named [RepositoryName]Impl. However, the modern best practice is to match the fragment name: UserRepoCustomImpl.
```java
public class UserRepoCustomImpl implements UserRepoCustom {
    @PersistenceContext EntityManager em; // You can inject the EM here!

    @Override
    public void complexReport() {
        // Your custom, imperative logic here
    }
}
```
3. Compose the Main Repository: Extend both the standard JPA interface and your custom interface.
```java
public interface UserRepository extends JpaRepository<User, Long>, UserRepoCustom {
    // You now have access to save(), findById(), AND complexReport()
}
```
The Mechanism: When Spring creates the Proxy for UserRepository, it notices UserRepoCustom. It scans the classpath for UserRepoCustomImpl, instantiates it as a Spring Bean, and "wires" it into the Proxy. When you call userRepo.complexReport(), the Proxy delegates that specific call to your implementation class.

# Entity Lifecycle & State Detection
This module addresses one of the most common "silent performance killers" in Spring Data.

## The save() Mystery
You might have noticed that CrudRepository does not have separate create() and update() methods. It only has save().
The Mechanism: Internally, the default implementation (SimpleJpaRepository) looks like this:
```java
@Transactional
public <S extends T> S save(S entity) {
    if (entityInformation.isNew(entity)) {
        em.persist(entity); // INSERT
        return entity;
    } else {
        return em.merge(entity); // UPDATE (mostly)
    }
}
```
## The "Is New" Strategy
How does Spring know if an entity is "New"? By default, it checks the @Id field.
- If ID is null (or 0): It assumes the entity is new. -> Calls persist.
- If ID is not null: It assumes the entity exists. -> Calls merge.

## The "Assigned ID" Problem
This default behavior works perfectly for Auto-Increment IDs (database-generated), because the ID is null until the DB assigns it.

However, consider this scenario: You decide to use UUIDs and generate them in your Java code (e.g., inside the constructor) so you can have the ID available before hitting the database.

```java
@Entity
class User {
    @Id
    String id = UUID.randomUUID().toString(); // ID is NEVER null!
}
```
The Consequence:
1. You create a new User. ID is already set.
2. You call repo.save(user).
3. Spring sees a non-null ID. It thinks "This is an update."
4. It calls em.merge().
5. JPA Behavior: merge() must return a managed instance. It first issues a SELECT query to see if that ID exists in the DB.
6. It finds nothing (because it's actually new), so it then issues the INSERT.

Result: Every time you create a user, you define a SELECT + INSERT instead of just an INSERT. You have doubled your database I/O overhead.

## The Solution: Persistable<ID>
To fix this, your Entity must implement the Persistable<ID> interface. This forces you to override isNew().
```java
@Entity
class User implements Persistable<String> {
    @Id String id;
    @Transient boolean isNew = true; // Flag to track state

    @Override
    public boolean isNew() {
        return isNew; // Manual control
    }

    @PostLoad
    @PostPersist
    void markNotNew() {
        this.isNew = false;
    }
}
```
Now, Spring checks your isNew() method instead of guessing based on the ID.

# Domain Events & Auditing
In a traditional "Service Layer" architecture, your Service creates an entity, saves it, and then triggers side effects.
The "Old" Way:
```java
public void registerUser(UserDto dto) {
    User user = new User(dto);
    userRepository.save(user);
    // Explicit side effect coupling
    emailService.sendWelcomeEmail(user);
    analyticsService.logRegistration(user);
}
```

Spring Data allows you to invert this using Domain Events. The philosophy is that an Aggregate Root (the Entity) should be responsible for registering the fact that something important happened (an "Event"), but not responsible for handling the side effect.

## The Mechanism: @DomainEvents
You can place specific annotations on your Entity class. When you call repository.save(), Spring Data inspects the entity, looks for these events, and publishes them to the Spring ApplicationContext automatically.
```java
@Entity
public class User {
    @Id @GeneratedValue
    private Long id;
    
    // A temporary holding area for events
    @Transient
    private final Collection<Object> domainEvents = new ArrayList<>();

    public void register() {
        // Logic for registration
        // ...
        // Queue the event internally
        this.domainEvents.add(new UserRegisteredEvent(this));
    }

    // 1. Spring calls this BEFORE the SQL write
    @DomainEvents
    public Collection<Object> domainEvents() {
        return Collections.unmodifiableCollection(domainEvents);
    }

    // 2. Spring calls this AFTER publishing to clear the list
    @AfterDomainEventPublication
    public void clearDomainEvents() {
        this.domainEvents.clear();
    }
}
```
The Listener (Decoupled):
```java
@Component
public class WelcomeEmailListener {
    @EventListener
    public void handleUserRegistered(UserRegisteredEvent event) {
        // Sends email
    }
}
```
Now, your Service code is simply:
```java
public void registerUser(UserDto dto) {
    User user = new User(dto);
    user.register(); // State change + Event creation
    userRepository.save(user); // Persistence + Event Publishing
}
```
If the save() fails (transaction rollback), the event publishing is usually rolled back as well (if using Transactional Events), ensuring consistency.
