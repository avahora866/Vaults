---
tags:
  - Spring
---
Besides throwing an exception in your methods you can define a method that will return a `responseEntity<JsonObject>` that will allow you more fine grain control on what code, message etc to be sent back when this exception is thrown
```java
@ExceptionHandler
public ResponseEntity<StudentErrorResponse> handleException(StudentNotFoundException exc)
	{
	StudentErrorResponse error = new StudentErrorResponse();
	
	error.setStatus(HttpStatus.NOT_FOUND.value());
	error.setMessage(exc.getMessage());
	error.setTimeStamp(System.curretnTiemMillis());
	
	return new ResponseEntity<>(error, HttpStatus.NOT_FOUND)
}

public class StudentErrorResponse
{
	private int status;
	private String message;
	private String timestamp;
}
```
Above code can only be used in its own [[Rest|rest]] controller **Cannot be used in other controllers**
# Global Exception Handling
## `@ControllerAdvice`
`@ControllerAdvice` is similar to an interceptor / filter. It is used for:
- Pre-process requests to controllers
- Post-process responses to handle exceptions
This is a usage of the **Spring AOP feature**

Simply move the above code into a class marked with the `@ControllerAdvice` annotation