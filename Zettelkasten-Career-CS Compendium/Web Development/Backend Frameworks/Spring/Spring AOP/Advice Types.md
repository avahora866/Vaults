---
tags:
  - Spring
---
# Before Advice

Runs before the method  
Commonly used in logging, security, transactions.

> Springs @Transactional actually uses AOP to beginTransaction / commitTransaction

# After returning advice

Runs after the method (successfull execution)
Can be used to post-process data alongside the other common use cases
## Access Return Value
To access the return value of the method execution you can add another attribute to the annotation
```java
@AfterReturning(
	pointcut="execution(* findAccounts(..))"
	returning="result"
)
public void afterReturningFindAccountAdice(JoinPoint theJoinPoint, List<Account> result) {
	...
}
```
*Note: The param names 'result' must match*
## Modifying data
```java
@AfterReturning(
	pointcut="execution(* findAccounts(..))"
	returning="result"
)
public void afterReturningFindAccountAdice(JoinPoint theJoinPoint, List<Account> result) {
	if (!result.isEmpty())
	{
		result.get(0).setName("Daffy Duck")
	}
}
```

# After throwing advice

Runs after method (If exception is thrown)
## Access Exception
To access the exception you can add another attribute to the annotation
 
```java
@AfterThrowing(
	pointcut="execution(* findAccounts(..))"
	throwing="theExec"
)
public void afterReturningFindAccountAdice(JoinPoint theJoinPoint, Throwable theExec) {
	...
}
```
*Note: parameter names must be the same as in `@AfterReturning`*
*Note: If you want to swallow the exception (Make sure the exception is never seen by the main program) you need to use `@Around` instead*

# After finally advice

Runs after the method (finally) (regardless if success or failiure)
Logging/Auditing is easiest here

# Around advice

Runs before and after method

- Can be use for analytics
- Swallow exceptions
- Other common use cases
The`@AroundAdvice` uses the ProcedingJoinPoint as well which is the handle to the target method
```java
@Aroud(	pointcut="execution(* findAccounts(..))")
public void afterReturningFindAccountAdice(ProcedingJoinPoint theProcedingJoinPoint) throws Throwable {
	//get begin timestamp (Before method call)
	long begin = System.currentTimeMillis();
	
	// The method can be called now
	Object result = theProcedingJoinPoint.proceed();
	
	/get end timestamp (After method call)
	long end = System.currentTimeMillis();
}
```
# Handeling the exception
```java
@Aroud(	pointcut="execution(* findAccounts(..))")
public void afterReturningFindAccountAdice(ProcedingJoinPoint theProcedingJoinPoint) throws Throwable {
	//get begin timestamp (Before method call)
	long begin = System.currentTimeMillis();
	
	Object result = null
		try {
			// The method can be called now
			Object result = theProcedingJoinPoint.proceed();
		} catch (Exception exc) {
			//log exception
			println("@AroundAdvice exception: "+ exc);
			
			//handle and give defualt fortune (result) .. use this approach with caution
			result = "Nothing exciting here. Move along!"
				
				//Or rethrow it
				throw exc
		}
	
	
	/get end timestamp (After method call)
	long end = System.currentTimeMillis();
}
```
*Note: If we swallow the exception the main program will never know there is a problem*