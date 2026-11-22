---
tags:
  - DesignPattern
---
The **Composite** pattern is a **structural** design pattern that lets you **compose objects into tree structures** to represent part-whole hierarchies. It allows clients to treat **individual objects and compositions of objects uniformly**.

# Intent

- Treat individual objects and compositions of objects uniformly.
- Represent part-whole hierarchies of objects.

# Motivation

Suppose you're creating a file system structure that includes both files and folders. A folder can contain files as well as other folders. You want to perform operations (e.g., display, delete) without having to worry whether you're working with a single file or a folder. The composite pattern makes this uniform treatment possible.

# Structure

```plaintext
     Component
    +-----------+
    | operation |
    +-----------+
         ^
         |
  +------+-------+
  |              |
Leaf         Composite
+-----+     +--------------+
| op  |     | children[]   |
+-----+     | + operation  |
            +--------------+
```

## Participants

- **Component**: Declares the interface for objects in the composition.
- **Leaf**: Represents leaf objects (no children) in the composition.
- **Composite**: Represents a group of `Component` objects. It implements child-related operations.

# Key Idea

A **Composite** contains other `Component` objects (which may be `Leaf` or `Composite`), and it implements the same interface as `Leaf`, so the client doesn't need to care which it's dealing with.

# Java Example

Let's model a file system where we can treat files and folders uniformly.

## Step 1: Component Interface

```java
interface FileSystemComponent {
    void display(String indent);
}
```

## Step 2: Leaf Class (File)

```java
class File implements FileSystemComponent {
    private String name;

    public File(String name) {
        this.name = name;
    }

    public void display(String indent) {
        System.out.println(indent + "- File: " + name);
    }
}
```

## Step 3: Composite Class (Folder)

```java
import java.util.ArrayList;
import java.util.List;

class Folder implements FileSystemComponent {
    private String name;
    private List<FileSystemComponent> children = new ArrayList<>();

    public Folder(String name) {
        this.name = name;
    }

    public void add(FileSystemComponent component) {
        children.add(component);
    }

    public void remove(FileSystemComponent component) {
        children.remove(component);
    }

    public void display(String indent) {
        System.out.println(indent + "+ Folder: " + name);
        for (FileSystemComponent child : children) {
            child.display(indent + "  ");
        }
    }
}
```

## Step 4: Client

```java
public class CompositePatternDemo {
    public static void main(String[] args) {
        FileSystemComponent file1 = new File("Resume.pdf");
        FileSystemComponent file2 = new File("Photo.png");

        Folder documents = new Folder("Documents");
        documents.add(file1);

        Folder pictures = new Folder("Pictures");
        pictures.add(file2);

        Folder root = new Folder("Home");
        root.add(documents);
        root.add(pictures);

        root.display("");
    }
}
```

## Output:

```
+ Folder: Home
  + Folder: Documents
    - File: Resume.pdf
  + Folder: Pictures
    - File: Photo.png
```

# Benefits

- **Uniformity**: You can treat both leaf and composite objects the same way.
- **Extensibility**: Easy to add new types of components.
- **Tree structure**: Naturally represents hierarchies (e.g., UIs, file systems).

# Drawbacks

- **Can be overkill** if you don’t need a part-whole hierarchy.
- **Difficult to restrict composite-specific behavior** (e.g., you can't prevent a leaf from adding children if the interface supports it).

# Use Cases

- GUI frameworks: widgets and containers.
- File systems: files and folders.
- Document structures: paragraphs, sections, characters.
- Organization hierarchies: employees and departments.

# Related Patterns

- **Decorator**: Has a similar structure but focuses on adding behavior rather than hierarchy.
- **Flyweight**: Can be used to share leaf objects in a composite structure.
- **Visitor**: Can be used to define new operations over a composite structure.