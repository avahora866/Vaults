---
tags:
  - Spring
---
module of code for a cross-curring concern (loggin, security, ..)
# Aspect Order
You cant really control the order of execution of singular advices so the recommendation is to seprate diffrent types of advices (logging, security, etc..) into their own seperate aspects, as you can control the order of execution of aspects.
We do this using the `@Order` annotation
```java
@Order(1)
@Aspect
public class MyCloudLogAspect {
	...
}
```
*Note: Lower numbers have higher precendence*
# @Order
- Lower numbers have higher precedence
- Range: Integer.MIN_VALUE to Integer.MAX_VALUE
	- Negative numbers are allowed
- Does not have to be consecutive (1, 4, 7)
*Note: If two aspects have the same order it will still compile and any other aspects with the order annotation will run as expected, however those two with the same order will be undefined and could be either that runs first*