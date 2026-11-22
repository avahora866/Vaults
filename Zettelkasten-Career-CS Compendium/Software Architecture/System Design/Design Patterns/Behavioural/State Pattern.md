---
tags:
  - DesignPattern
---
The **State** pattern is a **behavioral** design pattern that allows an object to change its behavior when its internal state changes. The object will appear to change its class, enabling it to behave differently based on its current state.

# Intent

- Allow an object to alter its behavior when its internal state changes.
- Avoid the need for complex conditional statements that depend on the object's state.
- Implement state-specific behavior in separate state classes rather than embedding it in the context class itself.

# Motivation

In situations where an object needs to behave differently depending on its internal state, managing these changes with conditional logic (e.g., `if` or `switch` statements) becomes complex and error-prone. The **State** pattern removes this complexity by encapsulating the state-specific behavior in separate state classes, which allows the object to switch states without modifying its own code.

# Structure

1. **Context**: Maintains a reference to a `State` object, which represents its current state.
2. **State**: Interface that defines a method for handling requests in a specific state.
3. **ConcreteState**: Each concrete state class implements the `State` interface and provides specific behavior for the context in that state.

```plaintext
            +----------+           +-----------+
            | Context  |           |   State   |
            +----------+           +-----------+
                 |                    /      \
       +----------------+          +----------------+
       | ConcreteState1 |          | ConcreteState2 |
       +----------------+          +----------------+
```

# Java Example

Let’s implement a **Traffic Light** system, where the traffic light changes color (state) and each color (state) behaves differently.

## Step 1: Define the State Interface

The `State` interface will define the behavior for each traffic light color.

```java
// Step 1: State interface
interface TrafficLightState {
    void changeLight(TrafficLight context);
}
```

## Step 2: Implement Concrete States

We will have three concrete states: `Red`, `Green`, and `Yellow`.

```java
// Step 2: ConcreteState1 - Red Light
class RedLight implements TrafficLightState {
    public void changeLight(TrafficLight context) {
        System.out.println("Red Light - Stop!");
        context.setState(new GreenLight()); // Change to Green after Red
    }
}
```

```java
// Step 3: ConcreteState2 - Green Light
class GreenLight implements TrafficLightState {
    public void changeLight(TrafficLight context) {
        System.out.println("Green Light - Go!");
        context.setState(new YellowLight()); // Change to Yellow after Green
    }
}
```

```java
// Step 4: ConcreteState3 - Yellow Light
class YellowLight implements TrafficLightState {
    public void changeLight(TrafficLight context) {
        System.out.println("Yellow Light - Slow down!");
        context.setState(new RedLight()); // Change to Red after Yellow
    }
}
```

## Step 3: Define the Context

The `TrafficLight` class will act as the context, which maintains the current state and delegates the `changeLight()` behavior to the current state.

```java
// Step 5: Context class
class TrafficLight {
    private TrafficLightState currentState;

    public TrafficLight() {
        currentState = new RedLight(); // Initial state is Red
    }

    public void setState(TrafficLightState state) {
        this.currentState = state;
    }

    public void changeLight() {
        currentState.changeLight(this);
    }
}
```

## Step 4: Client Code

The client code will simulate a traffic light changing between red, green, and yellow.

```java
// Step 6: Client
public class StatePatternDemo {
    public static void main(String[] args) {
        TrafficLight trafficLight = new TrafficLight();

        // Simulate the traffic light changing
        trafficLight.changeLight(); // Red -> Green
        trafficLight.changeLight(); // Green -> Yellow
        trafficLight.changeLight(); // Yellow -> Red
    }
}
```

## Output:

```
Red Light - Stop!
Green Light - Go!
Yellow Light - Slow down!
Red Light - Stop!
```

# Benefits

- **Encapsulation of behavior**: Each state-specific behavior is encapsulated in its own class, making it easier to maintain and extend.
- **No conditional logic**: Avoids the need for complex `if` or `switch` statements in the context class.
- **Easier state management**: Adding new states becomes simpler, as each new state can be added as a new class without modifying existing code.

# Drawbacks

- **Increased number of classes**: Each state requires its own class, which can lead to an increase in the number of classes in your system.
- **Potential overhead**: For systems with few states or limited state changes, the state pattern might introduce unnecessary complexity.

# Common Uses

- **Finite State Machines (FSMs)**: Systems that need to model different states with transitions between them, such as traffic lights, vending machines, and turnstiles.
- **UI Components**: Components that change appearance or behavior based on states (e.g., buttons, form fields).
- **Game Development**: Managing different game states like loading, playing, paused, and game over.
- **Workflow systems**: Where the system's state can change based on different conditions (e.g., order processing states).

# Java Built-in Support

In Java, while there is no direct built-in State pattern support, it is commonly used in conjunction with frameworks like **JavaFX** for managing the UI state or in **state machines** and **workflow engines**.