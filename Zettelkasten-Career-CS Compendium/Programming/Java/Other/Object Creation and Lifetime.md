---
tags:
  - FORMAT
  - Java
---
# Object Creation and Lifetime in Java

In Java, objects are created dynamically in memory, and their lifetime is managed by the **JVM's Garbage Collector (GC)**. Understanding how objects are created, allocated in memory, and eventually collected is crucial for optimizing performance.

---

## Object Creation in Java

### Steps of Object Creation

1. **Memory Allocation**: When an object is created using `new`, JVM allocates memory in the **heap**.  
2. **Initialization**: The constructor of the class is called to initialize the object.  
3. **Reference Assignment**: The reference variable stores the memory address of the allocated object.

### Example:

```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }
}

public class ObjectCreation {
    public static void main(String[] args) {
        Person p = new Person("John"); // Object created in heap
    }
}
```

* `p` is a reference stored in the **stack**.  
* The actual `Person` object is stored in the **heap**.

---

## JVM Memory Structure for Objects

The JVM divides memory into different regions:

### Heap Memory (Used for Object Storage)

The heap is divided into **generations** for efficient garbage collection:

* **Young Generation** (Eden \+ Survivor Spaces): Stores newly created objects.  
* **Old Generation (Tenured Space)**: Stores long-lived objects.  
* **Metaspace** (from Java 8 onwards): Stores class metadata, replacing the **PermGen**.

### Stack Memory (Stores Local Variables & Method Calls)

* Each thread has its own stack.  
* Stores method call frames, local variables, and references to objects in the heap.

### Other Memory Areas

* **Method Area**: Stores class structure and static variables.  
* **Native Method Stack**: Used by native methods (JNI).

---

## Object Lifetime in Java

### Stages of Object Lifetime

1. **Object Creation**: The object is allocated memory in the heap.  
2. **Object in Use (Reachable)**: As long as references exist, the object remains accessible.  
3. **Unreachable (Eligible for GC)**: If no references exist, the object becomes **eligible for garbage collection**.  
4. **Garbage Collection**: JVM removes the object to free memory.

---

## Garbage Collection and Object Lifetime

The **JVM Garbage Collector (GC)** manages memory automatically by removing unreachable objects.

### Generational Garbage Collection Process

1. **New objects** are created in the **Eden** space of the Young Generation.  
2. If the Eden space is full, a **Minor GC** occurs, moving surviving objects to a **Survivor Space**.  
3. Objects that survive multiple Minor GCs are promoted to the **Old Generation**.  
4. When the Old Generation fills up, a **Major GC (Full GC)** is triggered.

### GC Algorithms

* **Serial GC**: Single-threaded, suitable for small applications.  
* **Parallel GC**: Multi-threaded, used for high-throughput applications.  
* **G1 GC** (Garbage-First): Collects garbage in regions for low-pause performance.  
* **ZGC & Shenandoah GC**: Ultra-low latency garbage collectors.

### Tuning Garbage Collection for Object Lifetime

You can tune the GC based on object allocation patterns:

* `-Xms512m -Xmx1024m` → Set heap size.  
* `-XX:+UseG1GC` → Use G1 GC.  
* `-XX:NewRatio=2` → Adjust Young to Old Generation ratio.

---

### Summary

* **Object Creation**: Uses `new` to allocate memory in the heap.  
* **Memory Usage**: Objects are stored in the **Young Generation**, then promoted to the **Old Generation**.  
* **Garbage Collection**: Automatically removes unused objects, optimizing memory.  
* **Performance Tuning**: GC tuning helps improve application performance.

By understanding object creation and memory management, you can write **efficient Java applications** that minimize memory leaks and optimize performance. 🚀

