---
tags:
  - Spring
---
A predicate expression for where advice should be applied

The pointcut expression is used as a value in the advice annotation:
```java
@Aspect
@Component
public class MyDemoLogginAspect {
	@Before("execution(public void addAccount())")
	public void beforeAddAccountAdvice() {
		...
	}
}
```
The `execution(public void addAccount())` is the pointcut expression and specifies what method this advice will run before

Spring AOP Uuses the AOP pointcut expression language
# Pointcut types
## Execution pointcut
Applies to the excution of a given method

```java
execution(modifier-pattern? return-type-pattern declaring-type-pattern? method-name-pattern(param-pattern) throws-pattern)

//Matches addAccount in AccountDao class
@Before("execution(public void com.luv2Code.aopdemo.dao.AccountDao.addAccount())")

//Matches any addAccount() in any class
@Before("execution(public void addAccount())")
	
//Matches any method starting with add in any class
@Before("execution(public void add*())")
	
//Matches any method returning anyting with the name addAccount in any class
@Before("execution(public * addAccount())")	
// Modifier is optional	
@Before("execution(* addAccount())")	
	
//Match on any methods in a package
@Before("execution(* com.luv2code.aopdemo.dao.*.*(..))")

```
*Note: The patterns with ? are optional*
*Note: `declaring-type-pattern` = `com.luv2Code.aopdemo.dao.AccountDao`*

### Param-Pattern
- () - matches a method with no arguments
- (*) - matches a method with one argumanrt of any type
- (..) - matches a method with 0 or more argumaents of any type
```java
@Before("execution(* addAccount())")	

//Match addAcount methods that have the Account param
@Before("execution(* addAccount(com.luv2code.aopdemo.Account))")	
//Match addAcount methods that have 0 or many params
@Before("execution(* addAccount(..))")

```
# Pointcut declarations
A way to reuse pointcut expressions across multiple advices. 
- Simply create a method in your aspect with an empty return type and body. 
- Annotate it with the poitncut expression. 
- Use the method name in your advices pointcut expressions

```java
@Pointcut("execution(* com.luv2code/aopdemo.dao.*.*(..))")
private void forDaoPackage() {}

@Before("forDaoPackage()")
public void beforeAddAccountAdvice() {
	...
}
```
# Combining Pointcuts
This done by using Logical Operators:
- AND (&&)
- OR (||)
- NOT (!)
```java
@Before("expressionOne() && expressionTwo()")

@Before("expressionOne() || expressionTwo()")

@Before("expressionOne() && !expressionTwo()")
```

```java
//combine pointcut: include package ... exclude getter/setter
@Before("forDaoPackage() && !(getter() || setter())")

```

*Note: You can also use the pointcut combinations inside the pointcut declarations*