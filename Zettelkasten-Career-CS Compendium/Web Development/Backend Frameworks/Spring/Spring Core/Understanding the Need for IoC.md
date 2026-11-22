---
Reference:
  - https://www.marcobehler.com/guides/spring-framework#dependency-injection-basics
tags:
  - Spring
  - SECONDARY_BRANCH
---
- [[#Beans|Beans]]
- [[#Dependency Injection|Dependency Injection]]
	- [[#Dependency Injection#What is a dependency|What is a dependency]]
	- [[#Dependency Injection#Inversion Of Control|Inversion Of Control]]
- [[#Spring Dependency Injection|Spring Dependency Injection]]
	- [[#Spring Dependency Injection#ApplicationContext|ApplicationContext]]
		- [[#ApplicationContext#XML Configuration|XML Configuration]]
	- [[#Spring Dependency Injection#ComponentScan|ComponentScan]]
- [[#Spring AOP and proxies|Spring AOP and proxies]]

# Beans

[[Beans|Beans]] are objects that have been instantiated by the Spring IoC container
The number of beans instantiated by the [[IoC (Inversion of Control) and Dependency Injection (DI)|IoC container]] is by default one (Singleton) but can be modified by the `@Scope` annotation

- Scope("singleton") → Your bean will be a singleton, there will only be one instance.
- Scope("prototype") → Every time someone needs a reference to your bean, Spring will create a new one. (There’s a couple of caveats here, like injecting prototypes in singletons, though).
- Scope("session") → There will be one bean created for every user HTTP session.
- etc.

The `@Bean` annotation is typically used on methods inside the `@Configuration` class that return an instantiated Object

# Dependency Injection

## What is a dependency

A dependency is simply a piece of software that you want to call from your own code. So, your software depends on that other software  
In the context of Java this would be a class you want to use (Class B) from the current class (Class A). So this class (Class A) depends on another class (Class B).  
One way for a class (Class A) to depend on another is to instantiate that class (Class B) with the new keyword. There are some issues with this:

- If you need to use the dependency (Class B) in multiple scopes in the same class (Class A), you will need to instantiate the dependency (Class B) multiple times
- If you need the same dependency (Class B) in multiple classes you will need multiple instantiation of this class (Class B) again  
    You could have a global class that instantiates the dependency (Class B) and then use that class to get the dependency in whichever class you need.

```java
import com.mysql.cj.jdbc.MysqlDataSource;

public class Application {

    INSTANCE;

    private DataSource dataSource;

    public DataSource dataSource() {
        if (dataSource == null) {
            MysqlDataSource dataSource = new MysqlDataSource();
            dataSource.setUser("root");
            dataSource.setPassword("s3cr3t");
            dataSource.setURL("jdbc:mysql://localhost:3306/myDatabase");
            this.dataSource = dataSource;
        }
        return dataSource;
    }
}

public class ClassA {
    public void method()
    {
        Application.INSTANCE.dataSource
    }
}
```

This solves the two above issues:

- Class A that uses the dep does not have instantiate the dep
- The global class is a Singleton so that only one dependency needs to be instantiated.  

There are still issues however:
- Class A actively has to know where to get its dependencies (Class B's) from
- If your program gets bigger and you get more dependencies you will have a massive global class to instantiate all the dependencies. At this point you can split things up into more classes/factories

## Inversion Of Control

[[IoC (Inversion of Control) and Dependency Injection (DI)|Inversion Of Control]] solves the issues of Class A having to know where to get its dependency from. It does this by adding the dependency as a parameter in Class A's constructor. Now Class A does not need to instantiate Class B.

```java
import javax.sql.DataSource;

public class ClassA {

    private ClassB dataSource;

    public UserDao(ClassB dataSource) { // (1)
        this.dataSource = dataSource;
    }

    public User findById(Integer id) {
        try (Connection connection = dataSource.getConnection()) { // (2)
               PreparedStatement selectStatement = connection.prepareStatement("select * from users where id =  ?");
               // TODO execute the select etc.
        }
    }

    public User findByFirstName(String firstName) {
        try (Connection connection = dataSource.getConnection()) { // (2)
               PreparedStatement selectStatement = connection.prepareStatement("select * from users where first_name =  ?");
               // TODO execute the select etc.
        }
    }
}

```

Now whoever uses Class A needs to pass in a valid Class B. However whoever uses Class A will still need to call that global class to get Class B. Class A no longer needs to know where to get Class B from however whoever uses Class A does still. The problem is moved up one and we still have the issue of the global class being massive when you have a lot of dependencies  
This is what Spring Dependency Injection Container solves. It realises that Class A needs Class B. Constructs Class B and constructs Class A (With the Class B dependency)

# Spring Dependency Injection

## ApplicationContext

The [[Application Context|application context]] is who has control over all the classes (deps) and can manage them appropriately (create them with the necessary dependencies)  
[[Application Context|ApplicationContext]] will need to be constructed and one of the parameters given in its construction is a Config class (class marked with `@Configuration`). Inside the configuration class will be methods that return the instantiated dependency and are marked with `@Bean` (See above heading)

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Scope;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyApplicationContextConfiguration {

    @Bean
    @Scope("singleton")
    // @Scope("prototype") etc.
    public DataSource dataSource() {
        MysqlDataSource dataSource = new MysqlDataSource();
        dataSource.setUser("root");
        dataSource.setPassword("s3cr3t");
        dataSource.setURL("jdbc:mysql://localhost:3306/myDatabase");
        return dataSource;
    }
}
```

Actual Application

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyApplication {

    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(MyApplicationContextConfiguration.class);
        UserDao userDao = ctx.getBean(UserDao.class);
        // User user1 = userDao.findById(1);
        // User user2 = userDao.findById(1);
        DataSource dataSource = ctx.getBean(DataSource.class);
    }
}
```

### XML Configuration

Instead of using annotations you can specify beans using xml configuration however that isa legacy and is hardly in use now

## ComponentScan

Although now we dont have to specify what each class needs as a dependency (We dont have to specify that UserDao requires a data Source). We still have the problem of the configuration class getting too big once we add more dependencies.  
This is where `@ComponentScan, @Compnent, @Autowired` comes in.  
`@ComponentScan` is put on the classes marked with `@Configuration`. It scans all the java classes inside its current package as well as sub packages for classes with the `@Component` annotation. The `@Autowired` is used in the class marked with `@Component` to mark dependencies

```java
import javax.sql.DataSource;
import org.springframework.stereotype.Component;
import org.springframework.beans.factory.annotation.Autowired;

@Component
public class UserDao {

    private DataSource dataSource;
    
    @Autowired
    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

}
```

# Spring AOP and proxies

Under the hood Spring can return a [[Proxy Pattern|proxy]] of your dependency instead of the actual dependency. Eventually the [[Proxy Pattern|proxy]] will delegate to your dependency however if needed will perform actions before and after the delegation. The most popular example of this is `@Transactional` where when put on a class that is a dependency of another class. It will open a database connection before the deleagation and close it afterwards

> More specifically, Spring will, by default, create dynamic [Cglib proxies](https://github.com/cglib/cglib), that do not need an interface for proxying to work (like JDK’s internal proxy mechanism): Instead, Cglib can proxy classes through subclassing them on the fly. ( If you are unsure about the individual proxy patterns, read more about the [proxies on Wikipedia](https://en.wikipedia.org/wiki/Proxy_pattern). )