---
tags:
  - Spring
---
The `@InitBinder` annotation woks as a pre-processor. It will pre-process each web requests sento to our controller. The method annotated with `@InitBinder` is executed.
Define a method inside your controller that you want to be executed before each request to endpoint in that controller. Annotate said method with `@InitBinder`

```java
@InitBinder
public void initBinder (WebDataBinder dataBinder)
{
	StringTrimmerEditor stringTrimmerEditor = new StringTrimmerEditor(true);
	
	dataBinder.registerCustomEditor(String.class, stringTrimmerEditor)
}
```
`StringTrimmerEditor` is a class from Spring.