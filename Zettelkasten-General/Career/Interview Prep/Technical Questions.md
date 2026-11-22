---
tags:
  - Career
---
# Java

1. What is Java and why is it so popular?

Answer: Java is a high-level programming language that's easy to use and works on many different types of computers. It is popular because it can run on any device, is reliable and secure, and has lots of helpful tools. People use Java for web development, business applications, and mobile apps (especially Android), and it has a large, supportive community.

2. What are the main principles of Object-Oriented Programming (OOP) in Java?

Answer: The four main principles of OOP in Java are:

- Encapsulation: Bundling data and functions together inside a class.
- Inheritance: Sharing features and behavior from one class to another.
- Polymorphism: Treating objects differently based on their type or class.
- Abstraction: Hiding complex details and showing only the basics.

 3. What is the difference between JDK, JRE, and JVM?

Answer: While answering this Java interview questions remember:

- JDK (Java Development Kit): A set of tools for creating Java programs, including everything in the JRE plus extra development tools.

- JRE (Java Runtime Environment): The software needed to run Java programs, including the JVM and libraries, but not the development tools.
- JVM (Java Virtual Machine): The part that runs Java programs and ensures they work on any device.

 4. How does Java achieve platform independence?

Answer: You can answer this Java full stack developer interview questions like this- Java achieves platform independence by converting code into a universal format called bytecode. This bytecode runs on any device with a Java Virtual Machine (JVM), making Java programs portable and able to run on different computers and systems.

  5. Explain the concept of the Java Collections Framework

Answer: The Java Collections Framework is a group of interfaces and classes that provide various methods to handle and manipulate data collections. Key interfaces include List, Set, and Map. Implementations such as ArrayList, HashSet, and HashMap offer different ways to store and retrieve data efficiently.

If you want to practice more for the Java interview questions then you can consider enrolling in a Java Full Stack Certification course. It will teach you all about the basics and advanced Java programming language. As well as after completing the course make you perfect for the interviews by conducting some mock interviews.

  6. What is the difference between ArrayList and LinkedList?

Answer: This is one of the most common Java developer interview questions, you can answer this like this-

- ArrayList: Backed by a dynamic array, it provides fast random access but slow insertion and deletion operations, especially in the middle of the list.
- LinkedList: Implements a doubly-linked list, providing faster insertion and deletion but slower access times compared to ArrayList.

 7. What are Java exceptions and how do you handle them?

Answer: Java exceptions are events that disrupt the normal flow of the program. They are categorized into checked (compile-time) and unchecked (runtime) exceptions. Exceptions are handled using try, catch, finally, and throw statements. The try block contains code that might throw an exception, catch handles the exception, and finally contains code that executes regardless of whether an exception occurred.

 8. What is the difference between == and .equals() in Java?

Answer:

- ==: Compares references to check if two objects point to the same memory location.
- .equals(): Compares the content or state of two objects, provided the equals() method is properly overridden in the class.

 9. What is a static variable and method in Java?

Answer: This question is one of the best Java basic interview questions this question indicates the level of basic knowledge you have. So, you can answer this like:

- Static variable: Belongs to the class rather than instances of the class and is shared among all instances.
- Static method: This can be called without creating an instance of the class and can only access other static members of the class.

  10. Explain the concept of multithreading in Java

Answer: Multithreading allows concurrent execution of two or more threads, which can run in parallel. Threads share the same memory space but can execute independently. Java provides built-in support for multithreading via the Thread class and the Runnable interface.

 11. What is synchronization in Java?

Answer: Most of the people ask this Java interview questions, To check your basic knowledge. So the best answer for the following question is: Synchronization is a mechanism to control access to shared resources by multiple threads to avoid data inconsistency. It ensures that only one thread can access a resource at a time by using synchronized blocks or methods.

 12. What is the purpose of the final keyword in Java?

Answer: The final keyword can be used in several contexts:

- Final variable: Its value cannot be changed once initialized.
- Final method: Cannot be overridden by subclasses.
- Final class: Cannot be subclassed.

 13. Describe the Java memory model

Answer: The Java memory model defines how threads interact through memory and how they communicate changes to variables. It ensures the visibility of changes made by one thread to other threads and provides mechanisms for the safe publication of shared variables.

 14. What is garbage collection in Java?

Answer: Garbage collection is the process of automatically reclaiming memory occupied by objects that are no longer in use. In Java programming, the garbage collector plays a crucial role in managing memory allocation and deallocation, helping to prevent memory leaks and optimize memory usage. Understanding how the Java garbage collector works can be important for answering Java programming interview questions effectively.

 15. Explain the concept of Java Streams API

Answer: The Java Streams API provides a functional approach to process sequences of elements (collections) with operations like filter, map, reduce, and collect. It supports parallel processing and enables more readable and concise code.

 16. What are lambda expressions in Java?

Answer: Lambda expressions are a feature introduced in Java 8 that provides a concise way to represent anonymous functions. They are used primarily with functional interfaces to pass behavior as parameters and enable functional programming constructs.

 17. What is the role of the transient keyword in Java?

Answer: The transient keyword is used to mark a variable as not serializable. During serialization, transient variables are not included in the serialized representation, which helps prevent sensitive information from being stored. Understanding the transient keyword is important for Java interview questions, as it can be a common topic related to object serialization and data security in Java.

 18. Explain the use of annotations in Java.

Answer: Annotations provide metadata about the code but do not alter its behavior. They are used to provide information to the compiler or runtime environment, such as _@Override, @Entity, @Deprecated_, and custom annotations. Knowledge of annotations is often essential for Java coding interview questions, where candidates may be asked to explain their usage and benefits in Java development.

 19. What is Java Reflection?

Answer: Java Reflection allows you to inspect and manipulate classes, methods, and fields at runtime. It provides the ability to obtain information about objects and classes, dynamically invoke methods, and modify fields.

 20. What is the difference between String, StringBuilder, and StringBuffer?

Answer:

- String: _Immutable;_ once created, its value cannot be changed.
- StringBuilder: _Mutable;_ designed for single-threaded scenarios where string modification is required.
- StringBuffer: _Mutable;_ similar to StringBuilder but synchronized, making it suitable for multi-threaded environments.
# AWS
  
1. Explain what EC2 instance metadata is. How does an EC2 instance get its IAM access key and secret key?

EC2 instance metadata is a service accessible from within EC2 instances, which allows querying or managing data about a given running instance.

It is possible to retrieve an instance’s IAM access key by accessing the `iam/security-credentials/role-name` metadata category. This returns a temporary set of credentials that the EC2 instance automatically uses for communicating with AWS services.

2. What is an SQS dead-letter queue (DLQ), and what could you use it for?

SQS is Amazon’s fully managed distributed queue service. Queues are useful for handling communication between different services or other components in distributed systems.

A DLQ is a special queue for messages that could not be processed properly. These queues are specially used for debugging and troubleshooting applications.

3. There are four main AWS services related to CI/CD: CodeCommit, CodePipeline, CodeBuild, and CodeDeploy. Describe each of them.

AWS CodeCommit is essentially a managed service—i.e. Amazon manages and scales it behind the scenes for you, just like S3—for Git-based source control.

AWS CodeBuild is used to build, test, and generate artifacts—files that are generated from successful build steps—for deployment. This, too, is a managed service, doing provisioning and scaling automatically.

AWS CodeDeploy automates application deployments to several types of _compute resources_ such as EC2 instances or ECS clusters.

AWS CodePipeline is a continuous delivery service that allows automating and integrating build, test, and deploy processes.

 4. What are some differences between EKS, ECS, and Fargate?
EKS, or Amazon Elastic Container Service for Kubernetes, is Amazon’s managed Kubernetes service. This service provides a managed _Kubernetes control plane_ that’s highly available, i.e. they run multiple master nodes for you.

ECS, or Elastic Container Service, is Amazon’s container orchestration service. This service is used to manage containers and their lifecycle within clusters.

Fargate is an ECS launch type which allows you to run containers in serverless clusters. In other words, it allows you to run containers without needing to manage the underlying cluster infrastructure.

5. Suppose you have hired a team of DevOps freelancers to set up your infrastructure. You create an IAM group called “devops” and add the team to that group. After the team finishes setting your infrastructure up, they leave your project. What actions should you take?

You should delete only the user accounts from the developers, and keep the IAM group. It is possible that, in the future, you will want to hire more DevOps freelancers to change your infrastructure.

Keeping the IAM group will mean you kept the underlying set of policies, and therefore, future users can reuse those policies.

6. Explain the following CloudFormation template and what is wrong with it:

```yaml
Mappings: 
  RegionMap: 
    us-east-1: 
      "HVM64": "ami-0ff8a91507f77f867"
    us-west-1: 
      "HVM64": "ami-0bdb828fd58c52235"
    eu-west-1: 
      "HVM64": "ami-047bb4163c506cd98"
    ap-southeast-1: 
      "HVM64": "ami-08569b978cc4dfa10"
    ap-northeast-1: 
      "HVM64": "ami-06cd52961ce9f0d85"
Resources: 
  myEC2Instance: 
    Type: "AWS::EC2::Instance"
    Properties: 
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
      InstanceType: !Ref InstanceType
```

This CloudFormation template contains a `Mappings` section, which is useful for defining key-value pairs to be used in other parts of the template. In this case, the `RegionMap` is defining a mapping between region names and the respective AMI IDs of a virtual machine image in each of those regions.

In the `Resources` section, the `RegionMap` is then used to create an EC2 instance using the correct `ImageId`. However, the `InstanceType` property tries to use a parameter called `InstanceType`, which is missing. Parameters are useful for reusing templates, by allowing for those to be customized during stack updates or creations.

To fix this template, you could add the following section. It would allow entering an instance type and provide a default one:

```yaml
Parameters: 
  InstanceType: 
    Type: String
    Default: t2.micro
```

7. What are the current types of load balancers present on AWS? How do you choose which one to use?

There are currently three types of managed load balancers offered by AWS.

Application Load Balancer: This load balancer operates at Layer 7 of the OSI model. It can be used to load-balance HTTP and HTTPS applications and can invoke Lambda functions, among several other features.

Network Load Balancer: Used for extreme performance, this load balancer operates at Layer 4 of the OSI model. It can, therefore, load-balance any kind of TCP traffic and can handle large amounts of requests with low latency.

Classic Load Balancer: This load balancer operates at Layer 4 or Layer 7 of the OSI model. It is now mostly used for legacy applications that run on EC2-Classic since application load balancers provide more features.

8. Explain these Elastic Beanstalk deployment modes:

- All at once
- Immutable
- Rolling
- Rolling with additional batches
All at once: This is the fastest mode. In this case, Elastic Beanstalk will simply stop all the running instances and then will deploy the new version to these instances.

Immutable: In this mode, Elastic Beanstalk will create a new Auto Scaling Group and deploy the new instances there. Then, if the deployment succeeds, it will replace the old Auto Scaling Group with the newly created one.

Rolling: During the deployment, Elastic Beanstalk will deploy the new application version to some of the currently running instances, and repeat that process until all instances are running the same version. Using this mode, the application will run below capacity, but with zero downtime and no additional costs.

Rolling with additional batches: In this mode, Elastic Beanstalk will make sure there is no capacity reduction. It does so by first starting new instances with the new version of the application, and only then deploying the new version to the old instances.

9. Explain the S3 consistency model.

The AWS S3 service provides read-after-write consistency for `PUT`s of new objects, meaning that as soon as an object is written to an S3 bucket, it can be retrieved.

However, suppose we tried retrieving an object and received a “Not Found” response. Because it’s not found, we immediately add it. For a small period of time, we’ll be unable to retrieve it because the Not Found response will be cached. Therefore, in this case, S3 is only _eventually_ consistent. Deleting or updating existing objects are also eventually consistent operations.

10. Explain what this IAM Policy does:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:ListBucket"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::company-data"
    },
    {
      "Action": [
        "ecs:RunTask"
      ],
      "Effect": "Allow",
     "Condition": {
        "ArnEquals": {
          "ecs:cluster": "arn:aws:ecs:us-east-1:123456789012:cluster/prod"
        }
      },
      "Resource": "arn:aws:ecs:us-east-1:123456789012:task-definition/update-tables:"
    }
  ]
}
```

This IAM policy allows any IAM identity or AWS resource that has it attached to list all objects from the `company-data` bucket.

It also allows running any revision of the `update-tables` task definition on the `prod` ECS cluster.
# OOP
 1. Can you give a summary of OOPs?

An employer may ask a question like this to get a better understanding of your knowledge of OOPs. It also gives the interviewer an idea of how well you can explain technical concepts to non-technical people. Answer this question with a broad explanation of OOPs rather than describing specific concepts.Example answer: 'OOPs is an approach to programming that focuses on objects and their interactions. In OOPs, you can create objects and reuse them in different situations. This allows for more efficient and reliable code.

  2. What are the four basic concepts of OOPs?

An interviewer might ask this to determine whether you have a basic understanding of the core concepts of OOPs. People also refer to these as pillars and principles. Despite the question not explicitly asking for information on each concept, it's usually best to elaborate on them briefly.Example: 'The four concepts of OOPs are: encapsulation, which is the bundling of data within an object and the restriction of direct access to it; inheritance, allowing a class to inherit from another class; abstraction, which allows us to hide irrelevant data; and polymorphism, which enables objects to adopt multiple forms.

 3. Can you explain the usefulness of the four basic concepts of OOPs?

The interviewer wants to know whether you understand how and why these concepts are useful in OOPs. This question requires more information than a question that asks what the concepts are. When answering, explain what each concept enables in OOPs.Example: 'Each of the four concepts provides a different way to improve code. Encapsulation is useful because it enables the grouping of related variables into objects. This results in fewer parameters for functions, which reduces complexity and increases reusability. Abstraction is useful because it further reduces complexity by hiding the details and showing only the essentials. It also reduces the impact of change. Inheritance is useful because it helps to eliminate redundant code, and polymorphism is useful because it allows us to define one interface for multiple types of objects.

 4. How is OOPs different from procedural programming?

By asking candidates to distinguish between these two types of programming, interviewers can assess whether job seekers understand the basic differences between them. Knowing the differences between programming methods is important because it can aid in understanding their strengths and weaknesses. In your answer, you might first note a similarity and then explain the most noteworthy differences.Example: 'One similarity between the two types of programming is that they both involve a sequence of instructions for the computer to perform although OOPs and procedural programming are different in a few key ways. The most significant difference is that OOPs relies on objects, while procedural programming focuses primarily on functions.

 5. What would be the main classes if you had to design an online hotel booking system?

This type of question tests your ability to put your knowledge of OOPs into practice. Note that the question asks for the main classes, not all. In your answer, focus on the key classes of such a system. Examples of classes could be Room, Guest and Booking.Example: 'Some of the main classes I would use in an online hotel booking system would be Room, Guest, Address, Booking and Payment. If the system were for multiple hotels, I would also include a Hotel class.

  6. What are the rules for constructors in OOPs?

This question requires you to know what constructors are and what they do. To answer, give a brief definition of them. State the rule for constructors, which is that it's necessary for a constructor to have the same name as the class it's constructing.Example: 'Constructors are methods for creating new objects. There are different types of constructors, such as default constructors, parametric constructors and copy constructors. Two important rules for constructors are that they require the same name as the class they're constructing and that a Java constructor can't have a return type.

 7. Would you say Java is a pure object-oriented language?

By asking this, the interviewer wants to know your opinion on whether Java is a truly object-oriented language. Some people consider Java an object-oriented language, while others argue it isn't. You can answer this question by giving your opinion and backing it up with examples.Example: 'I'm of the opinion that Java isn't a pure object-oriented language. The reason I say this is because Java includes many things which aren't objects, such as primitive data types.

 8. What would you do if your manager asked you to design something for an industry or process you didn't understand?

This type of question can give insight into how you would approach a project when you're not familiar with the domain. A fundamental part of answering this question is to show that you would gather information to understand the tasks required of you. You can describe how to use that information to complete the project.Example: 'My first step in this situation would be to get the understanding I need. To do this, I would most likely conduct my own research. I would aim to answer questions such as, 'What's the design's intended function?' and 'How might the user interact with it?'. I might also ask a more experienced colleague or my supervisor for advice. Once I have a good understanding, I can create a solution.'
# Spring
1. What Is Spring Framework?

Spring is the most broadly used framework for the development of Java Enterprise Edition applications. Further, the core features of Spring can be used in developing any Java application.

We use its extensions for building various web applications on top of the Jakarta EE platform. We can also just use its dependency injection provisions in simple standalone applications.

2. What Are the Benefits of Using Spring?
Spring targets to make Jakarta EE development easier, so let’s look at the advantages:

- Lightweight – There is a slight overhead of using the framework in development.
- Inversion of Control (IoC) – Spring container takes care of wiring dependencies of various objects instead of creating or looking for dependent objects.
- Aspect-Oriented Programming (AOP) – Spring supports AOP to separate business logic from system services.
- IoC container – manages Spring Bean life cycle and project-specific configurations
- MVC framework – used to create web applications or RESTful web services, capable of returning XML/JSON responses
- Transaction management – reduces the amount of boilerplate code in JDBC operations, file uploading, etc., either by using Java annotations or by Spring Bean XML configuration file
- Exception Handling – Spring provides a convenient API for translating technology-specific exceptions into unchecked exceptions.

 3. What Spring Sub-Projects Do You Know? Describe Them Briefly.
- Core – a key module that provides fundamental parts of the framework, such as IoC or DI
- JDBC – enables a JDBC-abstraction layer that removes the need to do JDBC coding for specific vendor databases
- ORM integration – provides integration layers for popular object-relational mapping APIs, such as JPA, JDO and Hibernate
- Web – a web-oriented integration module that provides multipart file upload, Servlet listeners and web-oriented application context functionalities
- MVC framework – a web module implementing the Model View Controller design pattern
- AOP module – aspect-oriented programming implementation allowing the definition of clean method-interceptors and pointcuts

4. What Is Dependency Injection?
Dependency injection, an aspect of Inversion of Control (IoC), is a general concept stating that we do not create our objects manually but instead describe how they should be created. Then an IoC container will instantiate required classes if needed.

5. How Can We Inject Beans in Spring?
A few different options exist in order to inject Spring beans:

- Setter injection
- Constructor injection
- Field injection

The configuration can be done using XML files or annotations.

6. Which Is the Best Way of Injecting Beans and Why?

The recommended approach is to use constructor arguments for mandatory dependencies and setters for optional ones. This is because constructor injection allows injecting values to immutable fields and makes testing easier.

7. What Is the Difference Between BeanFactory and ApplicationContext?
_BeanFactory_ is an interface representing a container that provides and manages bean instances. The default implementation instantiates beans lazily when _getBean()_ is called.

In contrast, _ApplicationContext_ is an interface representing a container holding all information, metadata and beans in the application. It also extends the _BeanFactory_ interface, but the default implementation instantiates beans eagerly when the application starts. However, this behavior can be overridden for individual beans.

8. What Is a Spring Bean?
The Spring Beans are Java Objects that are initialized by the Spring IoC container.

9. What Is the Default Bean Scope in Spring Framework?

By default, a Spring Bean is initialized as a _singleton_.

10. How to Define the Scope of a Bean?

In order to set Spring Bean’s scope, we can use _@Scope_ annotation or “scope” attribute in XML configuration files. Note that there are five supported scopes:

- Singleton
- Prototype
- Request
- Session
- Global-session

11. Are Singleton Beans Thread-Safe?

No, singleton beans are not thread-safe, as thread safety is about execution, whereas the singleton is a design pattern focusing on creation. Thread safety depends only on the bean implementation itself.

12. What Does the Spring Bean Life Cycle Look Like?

First, a Spring bean needs to be instantiated based on Java or XML bean definition. It may also be required to perform some initialization to get it into a usable state. After that, when the bean is no longer required, it will be removed from the IoC container.

13. What Is the Spring Java-Based Configuration?
It’s one of the ways of configuring Spring-based applications in a type-safe manner. It’s an alternative to the XML-based configuration.

14. Can We Have Multiple Spring Configuration Files in One Project?

Yes, in large projects, having multiple Spring configurations is recommended to increase maintainability and modularity.

We can load multiple Java-based configuration files:

```java
@Configuration
@Import({MainConfig.class, SchedulerConfig.class})
public class AppConfig {
```

Or we can load one XML file that will contain all other configs:

```java
ApplicationContext context = new ClassPathXmlApplicationContext("spring-all.xml");
```

And inside this XML file we’ll have the following:

```xml
<import resource="main.xml"/>
<import resource="scheduler.xml"/>
```

15. What Is Spring Security?

Spring Security is a separate module of the Spring framework that focuses on providing authentication and authorization methods in Java applications. It also takes care of most of the common security vulnerabilities such as CSRF attacks.

To use Spring Security in web applications, we can get started with the simple annotation _@EnableWebSecurity_.

16. What Is Spring Boot?

Spring Boot is a project that provides a pre-configured set of frameworks to reduce boilerplate configuration. This way, we can have a Spring application up and running with the smallest amount of code.

17. Name Some of the Design Patterns Used in the Spring Framework?

- Singleton Pattern – singleton-scoped beans
- Factory Pattern – Bean Factory classes
- Prototype Pattern – prototype-scoped beans
- Adapter Pattern – Spring Web and Spring MVC
- Proxy Pattern – Spring Aspect-Oriented Programming support
- Template Method Pattern – _JdbcTemplate_, _HibernateTemplate_, etc.
- Front Controller – Spring MVC _DispatcherServlet_
- Data Access Object – Spring DAO support
- Model View Controller – Spring MVC

18. How Does the Scope Prototype Work?
Scope _prototype_ means that every time we call for an instance of the Bean, Spring will create a new instance and return it. This differs from the default _singleton_ scope, where a single object instance is instantiated once per Spring IoC container.

19. How to Get _ServletContext_ and _ServletConfig_ Objects in a Spring Bean?

We can do either by implementing Spring-aware interfaces. 

We could also use _@Autowired_ annotation on those beans:

```java
@Autowired
ServletContext servletContext;

@Autowired
ServletConfig servletConfig;
```

20. What Is a Controller in Spring MVC?
Simply put, all the requests processed by the _DispatcherServlet_ are directed to classes annotated with _@Controller_. Each controller class maps one or more requests to methods that process and execute the requests with provided inputs.

21. How Does the _@RequestMapping_ Annotation Work?

The _@RequestMapping_ annotation is used to map web requests to Spring Controller methods. In addition to simple use cases, we can use it for mapping of HTTP headers, binding parts of the URI with _@PathVariable,_ and working with URI parameters and the _@RequestParam_ annotation

22. What Is Spring _JdbcTemplate_ Class and How to Use It?

The Spring JDBC template is the primary API through which we can access database operations logic that we’re interested in:

- Creation and closing of connections
- Executing statements and stored procedure calls
- Iterating over the _ResultSet_ and returning results

In order to use it, we’ll need to define the simple configuration of _DataSource_:

```java
@Configuration
@ComponentScan("org.baeldung.jdbc")
public class SpringJdbcConfig {
    @Bean
    public DataSource mysqlDataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/springjdbc");
        dataSource.setUsername("guest_user");
        dataSource.setPassword("guest_password");
 
        return dataSource;
    }
}
```


23. How to Enable Transactions in Spring and What Are Their Benefits?

There are two distinct ways to configure _Transactions_ — with annotations or by using Aspect-Oriented Programming (AOP) — each with their advantages.

- Provide a consistent programming model across different transaction APIs such as JTA, JDBC, Hibernate, JPA and JDO
- Support declarative transaction management
- Provide a simpler API for programmatic transaction management than some complex transaction APIs such as JTA
- Integrate very well with Spring’s various data access abstractions

24. What Is Spring DAO?

Spring Data Access Object (DAO) is Spring’s support provided to work with data access technologies like JDBC, Hibernate and JPA in a consistent and easy way.

25. What Is Aspect-Oriented Programming (AOP)?

_Aspects_ enable the modularization of cross-cutting concerns such as transaction management that span multiple types and objects by adding extra behavior to already existing code without modifying affected classes.

26. What Are Aspect, Advice, Pointcut and JoinPoint in AOP?

- _Aspect_ – a class that implements cross-cutting concerns, such as transaction management
- _Advice_ – the methods that get executed when a specific _JoinPoint_ with matching _Pointcut_ is reached in the application
- _Pointcut_ – a set of regular expressions that are matched with _JoinPoint_ to determine whether _Advice_ needs to be executed or not
- _JoinPoint_ – a point during the execution of a program, such as the execution of a method or the handling of an exception

27. What Is Weaving?

According to the official docs, _weaving_ is a process that links aspects with other application types or objects to create an advised object. This can be done at compile time, load time, or runtime. Spring AOP, like other pure Java AOP frameworks, performs _weaving_ at runtime.

28. What Is Reactive Programming?
Reactive programming is about non-blocking, event-driven applications that scale with a small number of threads, with back pressure being a key ingredient that aims to ensure producers don’t overwhelm consumers.

These are the primary benefits of reactive programming:

- Increased utilization of computing resources on multicore and multi-CPU hardware
- Increased performance by reducing serialization

Reactive programming is generally event-driven, in contrast to reactive systems, which are message-driven. So, using reactive programming does not mean we’re building a reactive system, which is an architectural style.

However, reactive programming may be used as a means to implement reactive systems if we follow the Reactive Manifesto, which is quite vital to understand.

Based on this, reactive systems have four important characteristics:

- Responsive – The system should respond in a timely manner.
- Resilient – In case the system faces any failure, it should stay responsive.
- Elastic – Reactive systems can react to changes and stay responsive under varying workload.
- Message-driven – Reactive systems need to establish a boundary between components by relying on asynchronous message passing.

29. What Is Spring WebFlux?

Spring WebFlux is Spring’s reactive-stack web framework, and it’s an alternative to Spring MVC.

30. What Are the Mono and Flux Types?

The WebFlux framework in Spring Framework 5 uses Reactor as its async foundation.

This project provides two core types: _Mono_ to represent a single async value and _Flux_ to represent a stream of async values. They both also implement the _Publisher_ interface defined in the Reactive Streams specification.

_Mono_ implements _Publisher_ and returns 0 or 1 elements:

```java
public abstract class Mono<T> implements Publisher<T> {...}
```

And _Flux_ implements _Publisher_ and returns _N_ elements:

```java
public abstract class Flux<T> implements Publisher<T> {...}
```

By definition, the two types represent streams, and so they’re both lazy. This means nothing is executed until we consume the stream using the _subscribe()_ method. Both types are also immutable, so calling any method will return a new instance of _Flux_ or _Mono_

31. What Is the Use of _WebClient_ and _WebTestClient_?

_WebClient_ is a component in the new Web Reactive framework that can act as a reactive client for performing non-blocking HTTP requests. Since it’s reactive client, it can handle reactive streams with back pressure, and it can take full advantage of Java 8 lambdas. It can also handle both sync and async scenarios.

On the other hand, the _WebTestClient_ is a similar class that we can use in tests. Basically, it’s a thin shell around the _WebClient._ It can connect to any server over an HTTP connection. It can also bind directly to WebFlux applications using mock request and response objects, without the need for an HTTP server.

32. What Are the Disadvantages of Using Reactive Streams?

There are some major disadvantages to using reactive streams:

- Troubleshooting a Reactive application is a bit difficult, so be sure to check out our tutorial on debugging reactive streams for some handy debugging tips.
- There is limited support for reactive data stores since traditional relational data stores have yet to embrace the reactive paradigm.
- There’s an extra learning curve when implementing.

33. Is Spring 5 Compatible With Older Versions of Java?

In order to take advantage of Java 8 features, the Spring codebase has been revamped. This means older versions of Java cannot be used. So, the framework requires a minimum of Java 8.

34. How Does Spring 5 Integrate With JDK 9 Modularity?

In Spring 5, everything has been modularized. This way, we won’t be forced to import jars that may not have the functionalities we’re looking for.

Please have a look at our guide to Java 9 modularity for an in-depth understanding of how this technology works.

Let’s see an example to understand the new module functionality in Java 9 and how to organize a Spring 5 project based on this concept.

We’ll first create a new class that contains a single method to return a _String_ “HelloWorld”. We’ll place this within a new Java project — _HelloWorldModule_:

```java
package com.hello;
public class HelloWorld {
    public String sayHello(){
        return "HelloWorld";
    }
}
```

Then we create a new module:

```java
module com.hello {
    export com.hello;
}
```

Now let’s create a new Java Project, _HelloWorldClient_, to consume the above module by defining a module:

```java
module com.hello.client {
    requires com.hello;
}
```

The above module will be available for testing now:

```java
public class HelloWorldClient {
    public static void main(String[] args){
        HelloWorld helloWorld = new HelloWorld();
        log.info(helloWorld.sayHello());
    }
}
```

35. Can We Use Both Web MVC and WebFlux in the Same Application?
As of now, Spring Boot will only allow either Spring MVC or Spring WebFlux, as Spring Boot tries to auto-configure the context depending on the dependencies that exist in its classpath.

Also, Spring MVC cannot run on Netty. Moreover, MVC is a blocking paradigm and WebFlux is a non-blocking style. So, we shouldn’t be mixing both together because they serve different purposes.

# REST

1. What is REST?
REST (Representational State Transfer) is the most commonly used architectural style for building web services and APIs. In a RESTful architecture, resources are identified by URIs (Uniform Resource Identifiers), and operations are performed on those resources using standard HTTP methods. The state of a resource is represented in JSON or XML, which is transferred between the client and server in the HTTP request and response bodies.

2. What is the relationship between HTTP and REST?

HTTP, which stands for Hypertext Transfer Protocol, is the dominant protocol for transmitting data between clients and servers on the internet. HTTP operates on a stateless request-response model, in which the client sends a request to the server and the server responds accordingly—without any knowledge of the client’s previous requests. REST emphasizes standardized, stateless interactions between clients and servers, which makes HTTP the ideal choice for implementing RESTful principles.

3. What is the difference between REST and SOAP?

REST and SOAP are two different architectural styles for building APIs. REST is a highly flexible approach that uses standard HTTP methods to interact with resources that are represented in JSON or XML. In contrast, SOAP is an XML-based messaging protocol that is used to transfer data between distributed applications and systems, and it follows a very strict structure. REST is more popular than SOAP, but SOAP is still used in many enterprise-scale systems that require advanced security and error handling features.

Learn more about the differences between REST and SOAP.

4. What are HTTP headers, and which ones are most commonly used?

HTTP headers contain the metadata that is included in an API request or response. These key-value pairs provide essential details that help the API client and server communicate more effectively—and enable developers to customize and optimize the API’s behavior.

Some of the most common request headers are `Accept`, which defines the media types that the client is able to accept from the server; `Authorization`, which is used to send the client’s credentials to the server when the client is attempting to access a protected resource; and `Content-Type`, which defines the media type in the request body.

The server also includes HTTP headers in its response to the client. Some of the most common response headers include `Cache-Control`, which defines how the response should be cached, and `Set-Cookie`, which instructs the client to store a cookie with the specified attributes.

5. What is a resource in a RESTful context?

A REST resource is any object or group of objects that can be accessed through a dedicated API endpoint. For instance, a simple blog API may have an `author` resource. Individual authors may be accessed via the `/authors/:id` endpoint, while the collection of all authors may be accessed via the `/authors` endpoint.

In a well-designed RESTful API, resources and their associated endpoints should follow standard naming conventions and represent a logical entity that makes sense in the context of the application. Proper representation of resources is crucial for building an API that can be easily maintained and consumed.

6. What are some defining features of REST APIs?

REST APIs have several characteristics that promote efficiency, scalability, and interoperability. One key feature is statelessness, which means that every request from the client includes all necessary information, and the server doesn’t store any client-specific data. This behavior allows servers to handle client requests without managing complex session data, which simplifies server design and promotes horizontal scalability.

Another important aspect of REST APIs is the separation of concerns between the client and server. A REST API client is responsible for the user interface and user experience, while the API server handles request processing and resource management. This clear delineation ensures modularity and enables each component to evolve independently.

Finally, all REST APIs have a uniform interface in which endpoints are accessed with an established set of HTTP methods. This consistency promotes API integration], as any client that supports HTTP can use a RESTful API—regardless of its platform or programming language. The uniform interface also makes REST APIs easier to maintain and accelerates the onboarding process for new consumers.

 7. What is the difference between the PUT and PATCH methods?

The PUT and PATCH methods are both used to modify resources, but they handle updates differently. The PUT method works by replacing a resource with the data that is included in the request body. This means that any fields or properties not included in the request body are deleted, and any new fields or properties are added.

In contrast, the PATCH method is used for making partial updates to a resource. For instance, if you have a `movie` resource with fields for `title`, `year`, and `rating`, but you only want to update the `rating`, you could use the PATCH method to send a request that only includes the new value for the `rating` field. The rest of the resource would remain unchanged. This behavior reduces the amount of data that must be transferred, which makes PATCH more efficient than PUT.

8. What is idempotency in a RESTful context?

An HTTP method is considered idempotent if it will result in the same outcome no matter how many times it is executed. All read-only methods are idempotent, as are PUT and DELETE. However, POST and PATCH are not idempotent. POST is not idempotent because calling it multiple times will result in multiple resources being created. PATCH _can_ be idempotent, but it is not necessarily so. For instance, a PATCH request may increment a specific field every time it is called, which would modify the resource every time.

 9. What are the most common HTTP status codes?

HTTP status codes are three-digit numbers included in the server’s response that indicate the result of a client’s request. Some of the most common HTTP status codes include:

200 OK: The request was successful, and the response includes the requested data.

201 Created: The request was successful, and the server has created a new resource with the provided data.

204 No Content: The request was successful, but there is no additional data to send in the response. This code is often used in response to DELETE requests.

400 Bad Request: The server cannot understand the request due to a client error, malformed syntax, or invalid parameters.

401 Unauthorized: The client lacks valid credentials to access the requested data or perform the desired action.

403 Forbidden: The client does not have permission to access the requested resource, even with valid authentication credentials.

404 Not Found: The server cannot find the requested resource or endpoint.

500 Internal Server Error: The server encountered an unexpected issue.

503 Service Unavailable: The server is not ready to handle the request, often due to overload or maintenance.

504 Gateway Timeout: The server acting as a gateway or proxy did not receive a timely response from the upstream server.

10. What are the advantages and drawbacks of statelessness in REST?

Statelessness is a hallmark trait of REST APIs, and it has many benefits. REST API servers don’t need to manage session data, which allows them to handle a large number of concurrent requests and supports horizontal scaling. This also leads to streamlined server logic and faster response times.

Statelessness has a few drawbacks, as well. Requests to a REST API may have larger payloads, as the client must include all of the necessary information. This may have a negative impact on network performance. Statelessness may also require clients to handle retries in the case of network or server failures, as there is no server-side context to help with these error scenarios.
# Software Principles
1. What are the SOLID principles?

The SOLID principles are five design principles intended to make software designs more understandable, flexible, and maintainable. They are:

- Single Responsibility Principle (SRP)
- Open/Closed Principle (OCP)
- Liskov Substitution Principle (LSP)
- Interface Segregation Principle (ISP)
- Dependency Inversion Principle (DIP)

2. Can you explain the Single Responsibility Principle (SRP)?

The Single Responsibility Principle states that a class should have only one reason to change, meaning it should have only one job or responsibility. This principle aims to reduce the complexity of code by ensuring that each class addresses a specific aspect of the program's functionality. For example:

```
public class Report
{
    public string ReportType { get; set; }

    public void GenerateReport()
    {
        /Code to generate report/
    }

    public void SaveReport()
    {
        /Code to save report/
    }
}
```

Here, Report class violates SRP as it handles both generating and saving the report. To follow SRP:

```
public class ReportGenerator
{
    public void GenerateReport()
    {
        /Code to generate report/
    }
}

public class ReportSaver
{
    public void SaveReport()
    {
        /Code to save report/
    }
}
```

3. What is the Open/Closed Principle (OCP)?

The Open/Closed Principle states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means you should be able to add new functionality without changing existing code. For example:

```
public abstract class Shape
{
    public abstract double GetArea();
}

public class Circle : Shape
{
    public double Radius { get; set; }

    public override double GetArea() => Math.PI \ Radius \ Radius;
}

public class Rectangle : Shape
{
    public double Width { get; set; }

    public double Height { get; set; }

    public override double GetArea() => Width \ Height;
}
```

Here, you can add new shapes without modifying the existing code by extending the Shape class.

4. Can you describe the Liskov Substitution Principle (LSP)?

The Liskov Substitution Principle states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. This principle ensures that a subclass can stand in for its superclass. For example:

```
public class Bird
{
    public virtual void Fly()
    {
        /Flying logic/
    }
}

public class Eagle : Bird
{
    public override void Fly()
    {
        /Flying logic specific to Eagle /
    }
}

public class Ostrich : Bird
{
    public override void Fly()
    {
        throw new NotImplementedException();
    }
}
```

Here, Ostrich violates LSP because it can't fly. A better approach:

```
public abstract class Bird { }

public class FlyingBird : Bird
{
    public virtual void Fly()
    {
        /Flying logic /
    }
}

public class Eagle : FlyingBird
{
    public override void Fly()
    {
        /Flying logic specific to Eagle /
    }
}

public class Ostrich : Bird
{
    / Ostrich-specific logic /
}
```

5. What is the Interface Segregation Principle (ISP)?

The Interface Segregation Principle states that no client should be forced to depend on methods it does not use. Instead of one fat interface, many small, specific interfaces are preferred. For example:

```
public interface IWorker
{
    void Work();
    void Eat();
}
```

Here, the IWorker interface is too broad. To follow ISP:

```
public interface IWork
{
    void Work();
}

public interface IEat
{
    void Eat();
}

public class Worker : IWork, IEat
{
    public void Work()
    {
        /Work logic /
    }

    public void Eat()
    {
        /Eat logic /
    }
}
```

Now, classes can implement only the interfaces they need.

6. Can you explain the Dependency Inversion Principle (DIP)?

The Dependency Inversion Principle states that high-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces or abstract classes). Additionally, abstractions should not depend on details. Details should depend on abstractions. For example:

```
public class LightBulb
{
    public void TurnOn()
    {
        /Turn on logic /
    }

    public void TurnOff()
    {
        /Turn off logic /
    }
}

public class Switch
{
    private LightBulb _bulb;

    public Switch(LightBulb bulb)
    {
        _bulb = bulb;
    }

    public void Operate()
    {
        /Switch logic /
    }

}
```

Here, Switch depends on the LightBulb class, which violates DIP. To follow DIP:

```
public interface ILightBulb
{
    void TurnOn();
    void TurnOff();
}

public class LightBulb : ILightBulb
{
    public void TurnOn()
    {
        /Turn on logic /
    }

    public void TurnOff()
    {
        /Turn off logic /
    }
}

public class Switch
{
    private ILightBulb _bulb;

    public Switch(ILightBulb bulb)
    {
        _bulb = bulb;
    }

    public void Operate()
    {
        /Switch logic /
    }

}
```

Now, Switch depends on the ILightBulb interface, not the concrete LightBulb class.
# Databases

1. Can you explain the difference between normalisation and de-normalisation?
These are two opposite processes involved in working with databases. To answer this question, it's useful if you can describe what they mean and the respective benefits of each. This demonstrates that you understand the usefulness of both processes rather than just a simple definition.Example answer: 'Data normalisation and de-normalisation are opposite processes. When you normalise data, you divide a large dataset into various tables based on your needs. This is useful for reducing redundancy and inconsistency in the data in addition to improving its integrity. It can also save storage space compared to de-normalised data. Conversely, de-normalisation is when you combine various data tables into a single table. This has the benefit of making it faster to retrieve the data and requires less storage room but results in more redundancy and less integrity.'

2. How to Ensure Data Integrity in a Relational Database?

Ensuring data integrity involves using constraints and rules:

- Primary Keys to ensure unique records.  
- Foreign Keys to maintain referential integrity.  
- Unique Constraints to ensure all values in a column are unique.  
- Not Null Constraints to ensure a column cannot have NULL values.  
- Check Constraints to ensure data meets specific criteria.  
- Transactions to ensure a series of SQL operations are completed successfully as a unit.

3. What are the ACID Properties in a Database and Why are They Important?

ACID properties ensure reliable transaction processing:

- Atomicity: Ensures that all operations within a transaction are completed successfully; otherwise, the transaction is aborted.
- Consistency: Ensures that a transaction brings the database from one valid state to another.
- Isolation: Ensures that transactions are executed independently without interference.
- Durability: Ensures that the results of a transaction are permanently stored in the database, even in case of a system failure.

4. What is a Relational Database and How does It Differ from a NoSQL Database?

A relational database uses structured tables to store data, with predefined schemas and relationships (usually using SQL). It ensures data integrity through ACID properties and is suitable for complex queries and transactions.

A NoSQL database, on the other hand, is designed for unstructured or semi-structured data and can store data in various formats like key-value pairs, documents, or graphs. NoSQL databases are often more flexible and scalable, suitable for big data and real-time web applications, but they might not provide the same level of ACID compliance as relational databases.

5. Explain the Importance of Data Normalization.
Data normalization is the process of organizing data to minimize redundancy and improve data integrity. It involves dividing a database into smaller tables and defining relationships between them. Normalization is important because it:

- Reduces data duplication.  
- Ensures data consistency.  
- Simplifies the structure, making it easier to maintain and update.  
- Improves query performance by eliminating unnecessary data.

6. What are Stored Procedures and When would we Use Them?
Stored procedures are precompiled collections of SQL statements and optional control-of-flow statements stored under a name and processed as a unit. They are used to:

- Encapsulate complex SQL queries and business logic.  
- Improve performance by reducing network traffic (client-server round trips).  
- Enhance security by controlling access to data through parameterized queries.  
- Ensure consistency and reusability across multiple applications.  
- Stored procedures are particularly useful when performing repetitive tasks such as data validation, business rule enforcement, or batch processing.

7. Describe the Process of Database Normalization and Denormalization.
Normalization involves organizing database tables to reduce redundancy and improve data integrity. It typically follows these steps:

- First Normal Form (1NF): Ensure each table column contains atomic (indivisible) values.  
- Second Normal Form (2NF): Ensure that all non-key columns are fully dependent on the primary key.  
- Third Normal Form (3NF): Ensure that all columns are only dependent on the primary key and not on other non-key columns.

Denormalization is the process of combining normalized tables to improve read performance, often at the expense of write performance and increased redundancy. Denormalization is used when read performance is critical, and the application can handle data redundancy and potential update anomalies.

8. Explain the Concept of Database Indexing and its Importance in Query Performance.
Database indexing involves creating a data structure that improves the speed of data retrieval operations on a table at the cost of additional writes and storage space. Indexes are important because they:

- Speed Up Queries: Significantly reduce the time required to retrieve data by allowing the database to find rows more efficiently.  
- Support Sorting and Searching: Improve performance of operations involving sorting and searching, such as ORDER BY and WHERE clauses.  
- Enhance Join Performance: Speed up joins between tables by quickly locating matching rows

9. How to Join Multiple Tables to Create a Comprehensive Dataset for Analysis?

Joining multiple tables in SQL is typically done using different types of joins:
- Inner Join: Returns rows with matching values in both tables. 
- Left Join: Returns all rows from the left table and matched rows from the right table, with NULL for unmatched rows.  
- Right Join: Returns all rows from the right table and matched rows from the left table, with NULL for unmatched rows.  
- Full Outer Join: Returns rows when there is a match in either table, with NULL for unmatched rows.

10. What are the Performance Considerations When Qerying Large Datasets?

When querying large datasets, consider
- Indexing: Ensure appropriate indexes are in place to speed up query execution.  
- Partitioning: Use table partitioning to manage large tables more efficiently.  
- Query Optimization: Write efficient queries, avoid unnecessary calculations and joins.  
- Avoiding Select : Select only necessary columns to reduce data volume.  
- Batch Processing: Process data in batches to avoid overloading the system.  
- Caching: Use caching mechanisms to store frequently accessed data.  
- Database Configuration: Ensure the database is properly configured with adequate resources.

11. How to Handle Missing Data in a Database?
Handling missing data in a database can involve:

- Ignoring: Skipping rows with missing values during analysis.  
- Imputing: Replacing missing values with a default value, mean, median, or a value derived from other data.  
- Deletion: Removing rows or columns with a high percentage of missing values.  
- Using Placeholders: Marking missing values with a specific placeholder (e.g., NULL).