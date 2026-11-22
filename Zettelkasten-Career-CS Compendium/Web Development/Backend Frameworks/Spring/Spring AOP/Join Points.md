---
tags:
  - Spring
---
When to apply code during program execution
 **A way to access method paramenters when inside an advice.**

JoinPoit has the metadata for the method we are executing

```java
@Before("...")
public void beforeAddAccountAdvice(JoinPoint theJoinPoint) {
	//display the method signature: void com.luv2code.aopdemo.dao.AccountDao.addAccount(Account, boolean)
	MethodSignature methodSig = (MethodSignature) theJoinPoint.getSignature();
	
	//display the method arguments
	//get args
	Object[] args = theJoinPoint.getArgs();
	
	//loop through args
	for (Object tempArg : args) {
		//tempArg: com.luv2code.aopdemo.Account@1ce24091 / true
		tempArg
	}
	
}
```
# ProceedingJoinPoint
The`@AroundAdvice` uses the ProcedingJoinPoint as well which is the handle to the target method
```java
@Aroud(	pointcut="execution(* findAccounts(..))")
public void afterReturningFindAccountAdice(ProcedingJoinPoint theProcedingJoinPoint) {
	//get begin timestamp (Before method call)
	long begin = System.currentTimeMillis();
	
	// The method can be called now
	Object result = theProcedingJoinPoint.proceed();
	
	/get end timestamp (After method call)
	long end = System.currentTimeMillis();
}
```