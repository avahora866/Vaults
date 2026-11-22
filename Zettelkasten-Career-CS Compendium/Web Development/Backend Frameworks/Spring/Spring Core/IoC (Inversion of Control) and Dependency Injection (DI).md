---
tags:
  - Spring
  - SECONDARY_BRANCH
---
Inversion of Control is a software engineering principle which transfers the control of objects or portions of a program to a container or framework.

<mark style="background: #FFB86CA6;">In the case of spring we implement Inversion of Control (IoC) through Dependency Injection (DI)</mark>, which is the process where an object only defines its dependencies and does not create them. The object delegates the task of constructing the dependencies to an IoC container

<mark style="background: #FFB86CA6;">In the Spring Framework the interface</mark> [[Application Context| ApplicationContext and the BeanFactory]] <mark style="background: #FFB86CA6;">represents the IoC container</mark>

There are [[Application Context#ApplicationContext Implementations|several implementations]] of the ApplicationContext provided by Spring:

Stand Alone Applications
- AnnotationConfigApplicationContext
- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext 

Web Applications
- AnnotationConfigWebApplicationContext
- XmlWebApplicationContext

In order to <mark style="background: #FFB86CA6;">assemble beans, the container uses configuration metadata</mark>, which can be in the <mark style="background: #FFB86CA6;">form of XML configuration or annotations</mark>.
## Constructor Injection vs Field Injection

There are 2 ways to inject dependencies and whatever you use you should stick with it throughout the whole project. There are recommendations that field injection is harmful but in the end its your choice

## @Qualifier vs @Primary
When there are more than one type of a dependency i.e.: auto wiring a interface with multiple implementations.
You will receive a NoUniqueBeanDefinitionException indicating that more than one bean is available for auto wiring 
To fix this you can go one of two ways:
1. Giving a name to each bean by adding a string value to the @Component("barFormatter") annotation. Followed by adding the @Qualifier annotation to the autowired field specifying the name of the implementation we want
2. Putting the @Primary annotation on the component to specify the bean we want to use by default. We can use this in conjunction with the @Qualifier annotation when we want a specific bean but if we want a default bean set in every other case
3. You can also name the field of the autowired field to the bean we want. This will tell spring what type of bean you want with no need for any extra annotations

## Autowiring interfaces instead of concrete classes?
We autowired interfaces instead of concrete classes as it is part of the SOLID principles ( #TODO : Check and link). 
If we autowired the concrete class, in the future if we implement another version of the interface that we want to use instead, we will have to update wherever the concrete class has been used as a dependency. Whereas if we autowired the interface instead we can simply add the @Primary annotation to the new implementation 

## @Autowired vs @Resource vs @Inject
https://www.baeldung.com/spring-annotations-resource-inject-autowire