---
tags:
  - FORMAT
  - Java
---
Annotations in Java provide metadata about code that doesn't directly affect program logic but can be used by compilers, frameworks, or tools. They are widely used for configuration, documentation, and code analysis.

Annotations are typically used for:

* Code Generation  
* Runtime Processing  
* Validation  
* Documentation

---

# Built-in Meta-Annotations

Meta-annotations are special annotations used to define and customize the behavior of your custom annotations. Java provides the following meta-annotations:

## Retention

* Specifies how long the annotation is retained.  
* Defined using `@Retention` from `java.lang.annotation.Retention`.

**Values**:

* `RetentionPolicy.SOURCE` → Discarded during compilation (e.g., @Override).  
* `RetentionPolicy.CLASS` → Retained in the `.class` file but not available at runtime.  
* `RetentionPolicy.RUNTIME` → Available at runtime for reflection.

**Example**:

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String value();
}
```

---

## Target

* Specifies the possible elements the annotation can be applied to.  
* Defined using `@Target` from `java.lang.annotation.ElementType`.

**Values**:

* `ElementType.METHOD` → Methods  
* `ElementType.FIELD` → Fields  
* `ElementType.TYPE` → Classes, Interfaces, Enums  
* `ElementType.PARAMETER` → Method Parameters  
* `ElementType.CONSTRUCTOR` → Constructors

**Example**:

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target({ElementType.METHOD, ElementType.FIELD})
@interface ExampleAnnotation {}
```

---

## Inherited

* Allows an annotation to be inherited by subclasses.  
* Only works for class-level annotations.

**Example**:

```java
import java.lang.annotation.Inherited;

@Inherited
@interface InheritedAnnotation {}

@InheritedAnnotation
class Parent {}

class Child extends Parent {} // Child will inherit InheritedAnnotation
```

---

## Documented

* Indicates that an annotation should be included in Javadoc.

**Example**:

```java
import java.lang.annotation.Documented;

@Documented
@interface DocumentationAnnotation {
    String description();
}
```

---

# Creating Custom Annotations

You can create your own annotations using the `@interface` keyword.

**Example**:

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface MyCustomAnnotation {
    String author();
    int version() default 1;
}
```

**Using the Annotation**:

```java
class Demo {
    @MyCustomAnnotation(author = "John Doe", version = 2)
    public void display() {
        System.out.println("Display method called.");
    }
}
```

---

# Processing Annotations at Runtime

You can use Java Reflection to read and process annotations at runtime.

**Example**:

```java
import java.lang.reflect.Method;

public class AnnotationProcessor {
    public static void main(String[] args) throws Exception {
        Method method = Demo.class.getMethod("display");
        
        if (method.isAnnotationPresent(MyCustomAnnotation.class)) {
            MyCustomAnnotation annotation = method.getAnnotation(MyCustomAnnotation.class);
            System.out.println("Author: " + annotation.author());
            System.out.println("Version: " + annotation.version());
        }
    }
}
```

---

# Annotation Processing Tool (APT)

APT is a framework used for processing annotations during the compilation phase using annotation processors. You can generate code, documentation, or perform validation.

## Steps to Create an Annotation Processor:

1. Create a custom annotation.  
2. Implement an annotation processor by extending `AbstractProcessor`.  
3. Register the processor using the `@SupportedAnnotationTypes`.

---

## Example Annotation Processor

**Custom Annotation**:

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.TYPE)
public @interface GenerateInfo {
    String value();
}
```

**Annotation Processor**:

```java
import javax.annotation.processing.*;
import javax.lang.model.SourceVersion;
import javax.lang.model.element.*;

@SupportedAnnotationTypes("GenerateInfo")
@SupportedSourceVersion(SourceVersion.RELEASE_17)
public class InfoAnnotationProcessor extends AbstractProcessor {

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        for (Element element : roundEnv.getElementsAnnotatedWith(GenerateInfo.class)) {
            GenerateInfo annotation = element.getAnnotation(GenerateInfo.class);
            System.out.println("Processing: " + element.getSimpleName() + " with value: " + annotation.value());
        }
        return true;
    }
}
```

**Using the Annotation**:

```java
@GenerateInfo("This is a sample class")
public class SampleClass {}
```

---

# Use Cases of Annotations

* **Spring Framework**: Annotations like `@Autowired`, `@Component` for dependency injection.  
* **JUnit**: `@Test`, `@BeforeEach` for unit testing.  
* **Jackson**: `@JsonProperty` for JSON serialization.  
* **Lombok**: `@Getter`, `@Setter` for reducing boilerplate code.

---

# Conclusion

Annotations and annotation processing provide a powerful mechanism to add metadata and dynamically process code. Using meta-annotations like `@Retention`, `@Target`, `@Inherited`, and `@Documented`, you can create effective custom annotations and leverage them in different contexts. APT allows developers to create robust annotation processors, making it a crucial part of modern Java development.

