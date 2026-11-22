---
tags:
  - DesignPattern
---
The **Command pattern** is a **behavioral** design pattern that encapsulates a request as an object, thereby allowing you to parameterize clients with queues, requests, and operations.

# Intent

- Encapsulate a request as an object, separating the **invoker** of the request from the object that knows how to **execute** it.
- Supports features like **undo/redo**, **logging**, **transactional behavior**, and **queueing commands**.

# Structure

1. **Command Interface**: Declares the `execute()` method.
2. **ConcreteCommand**: Implements the `execute()` method by invoking the receiver's action.
3. **Receiver**: Knows how to perform the operation.
4. **Invoker**: Triggers the command's `execute()` method.
5. **Client**: Configures the command and associates it with the receiver and invoker.

```plaintext
Client → Invoker → Command → Receiver
```

# Java Example

Let’s implement a simple remote control that can turn a light on and off using the command pattern.

```java
// Step 1: Command interface
interface Command {
    void execute();
    void undo();
}
```

```java
// Step 2: Receiver
class Light {
    public void turnOn() {
        System.out.println("Light is ON");
    }

    public void turnOff() {
        System.out.println("Light is OFF");
    }
}
```

```java
// Step 3: Concrete commands
class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOn();
    }

    public void undo() {
        light.turnOff();
    }
}

class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOff();
    }

    public void undo() {
        light.turnOn();
    }
}
```

```java
// Step 4: Invoker
class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }

    public void pressUndo() {
        command.undo();
    }
}
```

```java
// Step 5: Client
public class CommandPatternDemo {
    public static void main(String[] args) {
        Light livingRoomLight = new Light();
        
        Command lightOn = new LightOnCommand(livingRoomLight);
        Command lightOff = new LightOffCommand(livingRoomLight);

        RemoteControl remote = new RemoteControl();

        remote.setCommand(lightOn);
        remote.pressButton();   // Output: Light is ON
        remote.pressUndo();     // Output: Light is OFF

        remote.setCommand(lightOff);
        remote.pressButton();   // Output: Light is OFF
        remote.pressUndo();     // Output: Light is ON
    }
}
```

# Benefits

- **Decouples** the object that invokes the operation from the one that knows how to perform it.
- Makes it easy to **add new commands** without changing existing code.
- Supports **undo/redo**, **macro commands**, **logging**, and **queueing**.

# Drawbacks

- Can result in a **large number of classes** for complex systems (one per command).
- Adds some **indirection**, which may be unnecessary for simple tasks.

# Common Uses

- GUI buttons and menu actions
- Transactional behavior (like in databases)
- Task scheduling
- Undo/redo operations
- Macro recording (e.g., executing a sequence of commands)