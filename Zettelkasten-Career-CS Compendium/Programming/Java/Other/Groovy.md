---
tags:
  - Java
---

-------------------------------------------------------------------------------
Groovy is basically like java but is a dynamic language whilst java is a static language

# Multi-Methods

In groovy, the methods whihc will be invoked are chosen at runtime. This is called runtime dispatch or multi-methods. It means that the method will be chosen based on the types of the arguments at runtime. In java, this is the opposite methods are chosen at compile time based on the declared types

```java
int method(String arg){
    return 1;
}

int method(Object arg){ 
    return 2; 
}

Object o = "Object"
int result = method(o)
```

Java would call the method that returns 2 as it will use the static information type, meaning o is declared as an object. Groovy will call the method that returns 1 as it will choose at runtime when the method is actually called. Since it is called with a String then the String version is called