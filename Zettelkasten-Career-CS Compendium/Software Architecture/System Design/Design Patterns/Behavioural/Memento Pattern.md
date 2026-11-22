---
tags:
  - DesignPattern
---
The **Memento** pattern is a **behavioral** design pattern that allows you to capture and externalize an object’s internal state so that it can be restored later, without violating encapsulation.

# Intent

- Save and restore the **previous state** of an object without exposing its internal structure.
- Provide a **snapshot** mechanism useful for features like **undo/redo**, **versioning**, or **checkpoints**.

# Structure

1. **Originator**: The object whose state needs to be saved and restored.
2. **Memento**: A value object that holds the state of the originator. Only the originator can access its internal state.
3. **Caretaker**: Manages the history of mementos and can ask the originator to restore a state.

```plaintext
Caretaker → Memento ↔ Originator
```

# Java Example

Let’s implement a simple text editor with undo functionality using the Memento pattern.

```java
// Step 1: Memento class (immutable)
class TextMemento {
    private final String state;

    public TextMemento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}
```

```java
// Step 2: Originator
class TextEditor {
    private String content;

    public void type(String words) {
        content += words;
    }

    public void set(String text) {
        content = text;
    }

    public String get() {
        return content;
    }

    public TextMemento save() {
        return new TextMemento(content);
    }

    public void restore(TextMemento memento) {
        content = memento.getState();
    }
}
```

```java
// Step 3: Caretaker
import java.util.Stack;

class History {
    private Stack<TextMemento> backups = new Stack<>();

    public void save(TextMemento memento) {
        backups.push(memento);
    }

    public TextMemento undo() {
        if (!backups.isEmpty()) {
            return backups.pop();
        }
        return null;
    }
}
```

```java
// Step 4: Client
public class MementoPatternDemo {
    public static void main(String[] args) {
        TextEditor editor = new TextEditor();
        History history = new History();

        editor.set("Hello");
        history.save(editor.save());

        editor.type(" World!");
        history.save(editor.save());

        editor.type(" More text...");
        System.out.println("Current content: " + editor.get());

        editor.restore(history.undo());
        System.out.println("After 1st undo: " + editor.get());

        editor.restore(history.undo());
        System.out.println("After 2nd undo: " + editor.get());
    }
}
```

## Output:

```
Current content: Hello World! More text...
After 1st undo: Hello World!
After 2nd undo: Hello
```

# Benefits

- **Encapsulation preserved**: Internal details of the originator are hidden from other objects.
- Enables powerful features like:
    - **Undo/redo**
    - **Snapshots/checkpoints**
    - **State versioning**

# Drawbacks

- **Memory overhead**: Storing too many mementos (especially for large states) can be expensive.
- **Complexity**: Managing memento histories and consistency requires care.
- **Serialization**: If object state is complex or involves resources, storing snapshots may need deep copying or serialization.

# Common Uses

- Undo/redo functionality in applications
- Save-game features in games
- Transactional state management
- Time-travel debugging (capturing states during execution)