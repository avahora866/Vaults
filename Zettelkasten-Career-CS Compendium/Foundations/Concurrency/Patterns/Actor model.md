---
tags:
  - Concurrency
  - FLASHCARD
---
The **Actor** pattern is a concurrency model that treats "actors" as the fundamental units of computation. An actor is an independent entity that encapsulates **state** and **behavior**. Actors communicate *exclusively* by passing asynchronous messages to one another. Each actor has a "mailbox" (a queue) to store incoming messages and processes them one at a time.

This model's core principle is **"share nothing."** By avoiding shared memory and locks, it eliminates the possibility of race conditions and deadlocks, making it easier to build highly concurrent and fault-tolerant systems.

# Explanation
This pattern is a complete alternative to traditional thread-and-lock concurrency. Instead of threads competing for shared data, actors are lightweight processes that own their data.

An actor can do only three things when it receives a message:

1. **Send messages:** Send a finite number of messages to other actors (whose addresses it knows).
2. **Create new actors:** Spawn a finite number of new actors.
3. **Change its own state:** Designate the behavior to be used for the *next* message it receives.

**Key Concepts:**
  * **No Shared State:** Actors never share their internal state with any other actor. To share information, they must send an *immutable* message containing a copy of the data.
  * **Mailbox:** Each actor has a private mailbox (a message queue). Senders add messages to this queue without blocking.
  * **Asynchronous & Non-blocking:** When an actor sends a message, it is a "fire and forget" operation. The sending actor does not wait for the message to be processed; it immediately continues its own work.
  * **Sequential Processing:** An actor processes messages from its mailbox *one at a time*. This is how it guarantees its internal state is safe without needing any locks.

This model is highly scalable and resilient. Because actors are isolated, a failure in one actor does not crash the entire system. A "supervisor" actor can detect the failure and restart the crashed actor.

# Code
Java does not have a native actor implementation. The most common way to use this pattern is with a library like **Akka**. This example shows the basic concepts using Akka.

*Note: This requires adding the Akka library (e.g., `akka-actor-typed_2_13`) to your project.*

```java
import akka.actor.typed.ActorRef;
import akka.actor.typed.ActorSystem;
import akka.actor.typed.Behavior;
import akka.actor.typed.javadsl.AbstractBehavior;
import akka.actor.typed.javadsl.ActorContext;
import akka.actor.typed.javadsl.Behaviors;
import akka.actor.typed.javadsl.Receive;

/**
 * A "Greeter" Actor. It encapsulates its own state (a greeting message)
 * and behavior (how to respond to messages).
 */
public class GreeterActor extends AbstractBehavior<GreeterActor.Command> {

    // --- Message Protocol ---
    // Defines the messages this actor can receive
    public interface Command {}

    // Message to tell the actor to greet someone
    public static final class Greet implements Command {
        public final String whom;
        public final ActorRef<Greeted> replyTo; // Where to send the reply

        public Greet(String whom, ActorRef<Greeted> replyTo) {
            this.whom = whom;
            this.replyTo = replyTo;
        }
    }

    // Message to reply back to the sender
    public static final class Greeted implements Command {
        public final String whom;
        public final ActorRef<Greet> from;

        public Greeted(String whom, ActorRef<Greet> from) {
            this.whom = whom;
            this.from = from;
        }
    }

    // --- State & Behavior ---
    
    // 1. This factory method is the "constructor" for the Actor
    public static Behavior<Command> create() {
        return Behaviors.setup(GreeterActor::new);
    }

    private GreeterActor(ActorContext<Command> context) {
        super(context);
    }

    // 2. This is the message handler. It defines the behavior.
    @Override
    public Receive<Command> createReceive() {
        return newReceiveBuilder()
            .onMessage(Greet.class, this::onGreet)
            .build();
    }

    // This method is called when a "Greet" message is processed
    private Behavior<Command> onGreet(Greet message) {
        System.out.println("GreeterActor: Hello, " + message.whom + "!");
        
        // 3. Send a reply message back to the sender
        message.replyTo.tell(new Greeted(message.whom, getContext().getSelf()));
        return this; // Keep the same behavior
    }
}

/**
 * A "Main" Actor that starts the interaction.
 */
class MainActor extends AbstractBehavior<GreeterActor.Greeted> {

    // 4. Factory method
    public static Behavior<GreeterActor.Greeted> create() {
        return Behaviors.setup(MainActor::new);
    }

    private MainActor(ActorContext<GreeterActor.Greeted> context) {
        super(context);
    }

    // 5. Message handler for "Greeted" messages
    @Override
    public Receive<GreeterActor.Greeted> createReceive() {
        return newReceiveBuilder()
            .onMessage(GreeterActor.Greeted.class, this::onGreeted)
            .build();
    }

    private Behavior<GreeterActor.Greeted> onGreeted(GreeterActor.Greeted message) {
        System.out.println("MainActor: Greeting received for " + message.whom);
        
        // After receiving the reply, we can shut down the system
        return Behaviors.stopped();
    }
}

public class ActorPatternDemo {
    public static void main(String[] args) {
        // 6. Create the "ActorSystem" - the runtime that manages the actors
        final ActorSystem<GreeterActor.Greeted> mainSystem =
            ActorSystem.create(MainActor.create(), "MainActorSystem");

        // 7. Create the Greeter actor
        // We get an "ActorRef" (an address), not the actor instance itself
        final ActorRef<GreeterActor.Command> greeter =
            mainSystem.systemActorOf(GreeterActor.create(), "Greeter");

        // 8. Send the first message to the Greeter
        // We include "mainSystem.getSelf()" as the "replyTo" address
        greeter.tell(new GreeterActor.Greet("World", mainSystem.getSelf()));
    }
}
```

## Code Explanation
1. **`GreeterActor.create()`**: We define a static factory method to create the actor's `Behavior`.
2. **`createReceive()` / `onGreet()`**: This is the core behavior. `createReceive()` registers message handlers. When a `Greet` message arrives in this actor's mailbox, the `onGreet` method is executed.
3. **`message.replyTo.tell(...)`**: The `GreeterActor` processes the `Greet` message and then sends a `Greeted` message back to the sender (whose address was included in the original message). This is an asynchronous, non-blocking call.
4. **`MainActor.create()`**: This is a separate actor that will initiate the conversation and receive the reply.
5. **`onGreeted()`**: This is the `MainActor`'s behavior. It waits for a `Greeted` message and then prints to the console.
6. **`ActorSystem.create(...)`**: This starts the Akka runtime and creates the first (main) actor. The system manages the threads, mailboxes, and message dispatching.
7. **`systemActorOf(...)`**: We *never* instantiate an actor with `new`. We ask the `ActorSystem` to create it. In return, we get an `ActorRef` (an actor reference, or address).
8. **`greeter.tell(...)`**: This is how we send a message. We `tell` the `greeter`'s address to process a new `Greet` message. The main thread does *not* wait; it fires this message and exits the `main` method. The `ActorSystem`'s threads will handle the entire interaction from here.