---
tags:
  - DesignPattern
---
The **Mediator** pattern is a **behavioral** design pattern that centralizes communication between objects in a system, promoting loose coupling by keeping objects from referring to each other explicitly.

# Intent

- Define an object that encapsulates how a set of objects interact.
- Promote **loose coupling** by preventing objects from referring to each other explicitly.
- Simplify communication and control logic between multiple components.

# Motivation

In a system where many objects interact directly with each other, the **interconnections can become complex** and hard to maintain. By introducing a mediator, all interactions go through a central object, which reduces dependencies and simplifies object behavior.

# Structure

1. **Mediator Interface**: Declares methods for communication between colleague objects.
2. **ConcreteMediator**: Implements communication logic between colleague objects.
3. **Colleague Interface**: Defines the common interface for components communicating through the mediator.
4. **ConcreteColleague**: Implements component behavior and delegates communication to the mediator.

```plaintext
ConcreteColleague1 →  
                   \
                    → Mediator ← ConcreteColleague2  
                   /
ConcreteColleague3 →
```

# Java Example

Let’s implement a chat room system where users (colleagues) send messages through a chat room (mediator).

```java
// Step 1: Mediator interface
interface ChatMediator {
    void sendMessage(String message, User sender);
    void addUser(User user);
}
```

```java
// Step 2: Concrete mediator
import java.util.ArrayList;
import java.util.List;

class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();

    public void addUser(User user) {
        users.add(user);
    }

    public void sendMessage(String message, User sender) {
        for (User user : users) {
            if (user != sender) {
                user.receive(message, sender);
            }
        }
    }
}
```

```java
// Step 3: Colleague class
abstract class User {
    protected ChatMediator mediator;
    protected String name;

    public User(ChatMediator mediator, String name) {
        this.mediator = mediator;
        this.name = name;
    }

    public abstract void send(String message);
    public abstract void receive(String message, User sender);
}
```

```java
// Step 4: Concrete colleagues
class ChatUser extends User {
    public ChatUser(ChatMediator mediator, String name) {
        super(mediator, name);
    }

    public void send(String message) {
        System.out.println(name + " sends: " + message);
        mediator.sendMessage(message, this);
    }

    public void receive(String message, User sender) {
        System.out.println(name + " receives from " + sender.name + ": " + message);
    }
}
```

```java
// Step 5: Client
public class MediatorPatternDemo {
    public static void main(String[] args) {
        ChatMediator chatRoom = new ChatRoom();

        User alice = new ChatUser(chatRoom, "Alice");
        User bob = new ChatUser(chatRoom, "Bob");
        User charlie = new ChatUser(chatRoom, "Charlie");

        chatRoom.addUser(alice);
        chatRoom.addUser(bob);
        chatRoom.addUser(charlie);

        alice.send("Hi everyone!");
        bob.send("Hello Alice!");
    }
}
```

## Output:

```
Alice sends: Hi everyone!
Bob receives from Alice: Hi everyone!
Charlie receives from Alice: Hi everyone!
Bob sends: Hello Alice!
Alice receives from Bob: Hello Alice!
Charlie receives from Bob: Hello Alice!
```

# Benefits

- **Loose coupling**: Components only know about the mediator, not each other.
- **Simplified communication**: Centralizes control and coordination.
- **Improved maintainability**: Changes to communication logic are localized.

# Drawbacks

- **Mediator can become complex**: If it has to manage many components and interactions, it may become a "god object."
- **Reduced flexibility**: All communication is funneled through the mediator, which might restrict direct optimizations.

# Common Uses

- GUI frameworks (e.g., dialog boxes coordinating controls)
- Messaging systems (chat, event buses)
- Air traffic control systems (planes communicate through a tower)
- MVC architecture (Controller acts as a mediator between Model and View)