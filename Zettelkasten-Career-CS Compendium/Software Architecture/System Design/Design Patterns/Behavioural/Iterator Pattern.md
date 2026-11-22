---
tags:
  - DesignPattern
---
The **Iterator** pattern is a **behavioral** design pattern that provides a way to access elements of a collection **sequentially** without exposing the underlying representation.

# Intent

- Provide a standard interface for **traversing** a collection.
- **Decouple** the traversal logic from the collection implementation.
- Allow **multiple traversals** to coexist independently.

# Structure

1. **Iterator Interface**: Defines methods for accessing elements in a sequence, typically `hasNext()` and `next()`.
2. **ConcreteIterator**: Implements the iterator interface and keeps track of the current position in the traversal.
3. **Aggregate Interface** (optional in Java): Defines a method to create an iterator.
4. **ConcreteAggregate (Collection)**: Implements the aggregate interface and returns a new iterator instance.

```plaintext
Client → Iterator → Collection (Aggregate)
```

# Java Support

Java provides built-in support via:

- `java.util.Iterator` interface with:
    - `boolean hasNext()`
    - `E next()`
    - `void remove()` (optional)

# Example: Custom Collection with Iterator

Let’s define a custom collection of strings with its own iterator.

```java
// Step 1: Custom iterator interface
interface MyIterator<T> {
    boolean hasNext();
    T next();
}
```

```java
// Step 2: Aggregate interface
interface MyIterable<T> {
    MyIterator<T> createIterator();
}
```

```java
// Step 3: Concrete aggregate
class NameCollection implements MyIterable<String> {
    private String[] names;
    private int size = 0;

    public NameCollection(int capacity) {
        names = new String[capacity];
    }

    public void add(String name) {
        if (size < names.length) {
            names[size++] = name;
        }
    }

    public MyIterator<String> createIterator() {
        return new NameIterator();
    }

    private class NameIterator implements MyIterator<String> {
        private int index = 0;

        public boolean hasNext() {
            return index < size;
        }

        public String next() {
            return names[index++];
        }
    }
}
```

```java
// Step 4: Client
public class IteratorPatternDemo {
    public static void main(String[] args) {
        NameCollection names = new NameCollection(5);
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");

        MyIterator<String> iterator = names.createIterator();

        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

## Output:

```
Alice
Bob
Charlie
```

# Benefits

- **Encapsulation**: Internal structure of the collection is hidden.
- **Multiple Iterators**: Can be created and used independently.
- **Flexibility**: Easy to add different kinds of iterators (e.g., reverse, filtered).

# Drawbacks

- May **add complexity** when you need different kinds of iterations.
- **Overhead**: Extra classes and logic for simple iteration tasks.

# Common Uses

- Any standard collection class in Java (`ArrayList`, `HashMap`, etc.)
- File readers, stream processing
- Tree, graph traversal
- Custom containers or data structures

Java also supports the **enhanced for-each loop** using the `Iterable` interface, which can be implemented in your custom collections to make them compatible with this syntax.