---
tags:
  - FORMAT
  - Java
---
# @Field

In Groovy, `@Field` is an annotation used in scripts to declare variables as instance fields instead of local script variables Normally, variables defined in a Groovy script are local to the script's `run` method, meaning they are not accessible across methods within the script. The `@Field` annotation solves this by promoting a script variable to an instance field, making it accessible throughout the script.

### Example Without \`@Field\`

\`\`\`groovy  
int counter \= 0

def increment() {  
    counter++ // Throws MissingPropertyException: No such property: counter  
}

increment()  
println counter  
\`\`\`  
This will fail because the counter is local to the run method and isn't visible inside `increment()`.

### Example With `@Field`

\`\`\`groovy  
import groovy.transform.Field

@Field int counter \= 0

def increment() {  
    counter++  // Now accessible  
}

increment()  
println counter  // Outputs: 1  
\`\`\`  
Here, counter is accessible inside \`increment()\` because `@Field` makes it a field of the script class.

### Key Points:

\- `@Field` promotes a script variable to an instance field, making it accessible in script methods.  
\- Useful in scripts where you want to maintain state across multiple method calls.  
\- It is only needed in script files, not in normal Groovy classes.

# Groovy Script run method

### Groovy Script's `run` Method Explained

In Groovy, when you write a script (a `.groovy` file that is not explicitly a class), the script is compiled into a class that extends `groovy.lang.Script`. This class has a special method called `run()`, which acts as the **entry point** of the script.

---

### How `run()` Works

* Every Groovy script is implicitly converted into a class that extends `groovy.lang.Script`.  
* The script’s body is placed inside the `run()` method.  
* When you execute the script, the Groovy runtime **calls `run()`**, executing the script’s body.

#### Example 1: Implicit `run()` Method

```
println "Hello, Groovy!"
```

Groovy compiles this into:

```
public class Script123 extends groovy.lang.Script {
    public Object run() {
        System.out.println("Hello, Groovy!");
        return null;
    }
}
```

When you run the script, Groovy calls `run()`, printing **"Hello, Groovy\!"**.

---

### Defining a Custom `run()` Method

You can override `run()` if you need more control:

```
class MyScript extends Script {
    @Override
    Object run() {
        println "Custom run method"
        return "Done"
    }
}

def script = new MyScript()
println script.run() // Outputs: Custom run method
```

Here, we manually create a script class and override `run()`.

---

### Interaction with Methods

Since the script’s body is inside `run()`, any variables declared inside it are **local to `run()`**. That’s why we use `@Field` to make them instance fields.

```
int count = 0

def increment() {
    count++ // Error! count is local to run()
}

increment()
println count
```

This fails because `count` is a local variable inside `run()`, so `increment()` can't see it.

Using `@Field` fixes this:

```
import groovy.transform.Field

@Field int count = 0

def increment() {
    count++
}

increment()
println count // Works! Outputs: 1
```

---

### Key Takeaways

1. **Implicit Execution**: The script body runs inside `run()`, even if you don’t explicitly define it.  
2. **Script as a Class**: Groovy scripts are compiled into classes extending `groovy.lang.Script`.  
3. **Variable Scope**: Variables in the script body are local to `run()`, so methods can’t see them unless you use `@Field`.  
4. **Custom `run()`**: You can override `run()` if you need explicit control.