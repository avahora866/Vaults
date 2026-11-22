---
tags:
  - Java
---
It's purpose is to replace the POJO but to be more restrictive i.e:
- You can only set the values once in the constructor (no Setter method)
- The fields generated are private final 

It reduces the boiler plate code of getters, setters, constructors and field declaration by creating one line a record header

```java
public record Student(Long id, String firstName, String lastName) {}
```

This tells java to create 
- 3 private final fields
- an all args constructor
- `toString` method
- equals method (check?)
- `hashCode` method (check?) 
In one line

There are no Setter methods generated as the record seeks to create immutability for its fields

# Why would you want the record to be immutable?
If you are simply reading from a database or from a file, you don't want any fields changed by the developer by mistake. This allows you to make sure the instant you are handling is straight from the file and has not been modified in any way.