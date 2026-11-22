---
tags:
  - Architecture
  - FORMAT
---
This article will focus on **simplifying the DAO layer** by using a single, gentrified Data Access Object for all entities in the system, which will result in elegant data access, with no unnecessary clutter or verbosity.  
We'll build on the Abstract DAO class we saw in [our previous article](https://www.baeldung.com/persistence-layer-with-spring-and-hibernate) on Spring and Hibernate, and add generics support.

The Data Access Object (DAO) pattern is a structural pattern that allows us to **isolate the application/business layer from the persistence layer (usually a relational database, but could be any other persistence mechanism) using an abstract API.** The API hides from the application all the complexity of performing CRUD operations in the underlying storage mechanism. This permits both layers to evolve separately without knowing anything about each other.

To understand how the DAO pattern works, let's create a basic example.  
Let's say that we want to develop an application that manages users. We want to keep the application's domain model completely agnostic about the database. So, we'll create **a simple DAO class that will take care of keeping these components neatly decoupled from each other.**

# The Domain Class

As our application will work with users, we need to define just one class for implementing its domain model:

```java
public class User {

private String name;
private String email;

    // constructors / standard setters / getters
}
```

The *User* class is just a plain container for user data, so it doesn't implement any other behaviour worth stressing.  
Of course, the important design choice here is how to keep the application using this class isolated from any persistence mechanism that could be implemented.  
And that's exactly the issue that the DAO pattern attempts to address.

# The DAO API

Let's define a basic DAO layer, so we can see how it can **keep the domain model completely decoupled from the persistence layer.**

Here's the DAO API:

```java
public interface Dao<T> {

    Optional<T>get(long id);

    List<T>getAll();

void save(T t);

void update(T t, String[] params);

void delete(T t);
}
```

From a bird's-eye view, it's clear that the *Dao* interface defines an abstract API that performs CRUD operations on objects of type *T*.

Due to the high level of abstraction that the interface provides, it's easy to create a concrete, fine-grained implementation that works with *User* objects.

# The UserDao Class

Let's define a user-specific implementation of the *Dao* interface:

```java
public class UserDaoimplementsDao<User> {

private List<User> users =newArrayList<>();

public UserDao() {
        users.add(newUser("John", "john@domain.com"));
        users.add(newUser("Susan", "susan@domain.com"));
    }

    @Override
public Optional<User>get(long id) {
return Optional.ofNullable(users.get((int) id));
    }

    @Override
public List<User>getAll() {
return users;
    }

    @Override
public void save(User user) {
        users.add(user);
    }

    @Override
public void update(User user, String[] params) {
        user.setName(Objects.requireNonNull(
          params[0], "Name cannot be null"));
        user.setEmail(Objects.requireNonNull(
          params[1], "Email cannot be null"));

        users.add(user);
    }

    @Override
public void delete(User user) {
        users.remove(user);
    }
}
```

The *UserDao* class implements all the functionality required for fetching, updating and removing *User* objects.

**For simplicity's sake, the *users List* acts like an in-memory database, which is populated with a couple of *User* objects in the constructor.**

Of course, it's easy to refactor the other methods, so they can work, for instance, with a relational database.

While both the *User* and *UserDao* classes coexist independently within the same application, we still need to see how the latter can be used for keeping the persistence layer hidden from application logic:

```java
public classUserApplication {

private static Dao<User> userDao;

public static void main(String[] args) {
        userDao =newUserDao();

User user1 = getUser(0);
        System.out.println(user1);
        userDao.update(user1,newString[]{"Jake", "jake@domain.com"});

User user2 = getUser(1);
        userDao.delete(user2);
        userDao.save(newUser("Julie", "julie@domain.com"));

        userDao.getAll().forEach(user -> System.out.println(user.getName()));
    }

private static User getUser(long id) {
        Optional<User> user = userDao.get(id);

return user.orElseGet(
          () ->newUser("non-existing user", "no-email"));
    }
}
```

The example is contrived, but it shows in a nutshell the motivations behind the DAO pattern. In this case, the *main* method just uses a *UserDao* instance to perform CRUD operations on a few *User* objects.

**The most relevant facet of this process is how *UserDao* hides from the application all the low-level details on how the objects are persisted, updated and deleted.**

# Using the Pattern With \[\[JPA (Jakarta Persistence API (formally Java Persistence API)) Needs Updating\]\]

There's a tendency among developers to think that the release of JPA downgraded to zero the DAO pattern's functionality. The pattern becomes just another layer of abstraction and complexity on top of the one provided by JPA's entity manager.

This is true in some scenarios. Even so, **we sometimes just want to expose to our application only a few domain-specific methods of the entity manager's API.** The DAO pattern has its place in such cases.

## The JpaUserDao Class

Let's create a new implementation of the *Dao* interface to see how it can encapsulate the functionality that JPA's entity manager provides out of the box:

```java
public class JpaUserDao implements Dao<User> {

private EntityManager entityManager;

    // standard constructors

    @Override
public Optional<User>get(long id) {
return Optional.ofNullable(entityManager.find(User.class, id));
    }

    @Override
public List<User>getAll() {
Query query = entityManager.createQuery("SELECT e FROM User e");
return query.getResultList();
    }

    @Override
public void save(User user) {
        executeInsideTransaction(entityManager -> entityManager.persist(user));
    }

    @Override
public void update(User user, String[] params) {
        user.setName(Objects.requireNonNull(params[0], "Name cannot be null"));
        user.setEmail(Objects.requireNonNull(params[1], "Email cannot be null"));
        executeInsideTransaction(entityManager -> entityManager.merge(user));
    }

    @Override
public void delete(User user) {
        executeInsideTransaction(entityManager -> entityManager.remove(user));
    }

private void executeInsideTransaction(Consumer<EntityManager> action) {
EntityTransaction tx = entityManager.getTransaction();
try {
            tx.begin();
            action.accept(entityManager);
            tx.commit();
        }
catch (RuntimeException e) {
            tx.rollback();
throw e;
        }
    }
}
```

**The *JpaUserDao* class can work with any relational database supported by the JPA implementation.**

Also, if we look closely at the class, we'll realize how the use of [Composition](https://en.wikipedia.org/wiki/Composition_over_inheritance) and [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection) allows us to call only the entity manager methods required by our application.

Simply put, we have a domain-specific tailored API, rather than the entire entity manager's API.

## Refactoring the User Class

In this case, we'll use Hibernate as the JPA default implementation, so we'll refactor the *User* class accordingly:

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
private long id;

private String name;
private String email;

    // standard constructors / setters / getters
}

```

## Bootstrapping a JPA Entity Manager Programmatically

Assuming that we already have a working instance of MySQL running either locally or remotely and a database table *“users”* populated with some user records, we need to get a JPA entity manager, so we can use the *JpaUserDao* class for performing CRUD operations in the database.

In most cases, we accomplish this via the typical *persistence.xml* file, which is the standard approach.

In this case, we'll take an XML-less approach and get the entity manager with plain Java through Hibernate's handy *[EntityManagerFactoryBuilderImpl](https://docs.jboss.org/hibernate/orm/5.0/javadocs/org/hibernate/jpa/boot/internal/EntityManagerFactoryBuilderImpl.html)* class.

For a detailed explanation on how to bootstrap a JPA implementation with Java, please check [this article](https://www.baeldung.com/java-bootstrap-jpa)

## The UserApplication Class

Finally, let's refactor the initial *UserApplication* class, so it can work with a *JpaUserDao* instance and run CRUD operations on the *User* entities:

```java
public class UserApplication {

private static Dao<User> jpaUserDao;

    // standard constructors

public static void main(String[] args) {
User user1 = getUser(1);
        System.out.println(user1);
        updateUser(user1,newString[]{"Jake", "jake@domain.com"});
        saveUser(newUser("Monica", "monica@domain.com"));
        deleteUser(getUser(2));
        getAllUsers().forEach(user -> System.out.println(user.getName()));
    }

public static User getUser(long id) {
        Optional<User> user = jpaUserDao.get(id);

return user.orElseGet(
          () ->newUser("non-existing user", "no-email"));
    }

public static List<User>getAllUsers() {
return jpaUserDao.getAll();
    }

public static void updateUser(User user, String[] params) {
        jpaUserDao.update(user, params);
    }

public static void saveUser(User user) {
        jpaUserDao.save(user);
    }

public static void deleteUser(User user) {
        jpaUserDao.delete(user);
    }
}
```

The example here is pretty limited. But it's useful for showing how to integrate the DAO pattern's functionality with the one that the entity manager provides.

In most applications, there's a DI framework, which is responsible for injecting a *JpaUserDao* instance into the *UserApplication* class. For simplicity's sake, we've omitted the details of this process.

The most relevant point to stress here is how **the *JpaUserDao* class helps to keep the *UserApplication* class completely agnostic about how the persistence layer performs CRUD operations.**

In addition, we could swap MySQL for any other RDBMS (and even for a flat database) further down the road, and our application would continue working as expected, thanks to the level of abstraction provided by the *Dao* interface and the entity manager.