---
tags:
  - DesignPattern
---
The **Observer** pattern is a **behavioral** design pattern that defines a **one-to-many** dependency between objects so that when one object (the **subject**) changes state, all its dependents (**observers**) are notified and updated automatically.

# Intent

- Allow multiple objects to observe and react to changes in another object.
- Loosely couple the subject and its observers.

# Motivation

Instead of tightly coupling objects together where one has to call the others directly, you decouple the logic: the subject only knows that "something" is interested in its state, and the observers just subscribe and react when notified.

# Structure

1. **Subject**: Maintains a list of observers and notifies them of changes.
2. **Observer Interface**: Defines the `update()` method.
3. **ConcreteObserver**: Implements the `update()` method and reacts to notifications.
4. **ConcreteSubject**: Stores state and calls `notifyObservers()` when that state changes.

```plaintext
            +-------------+
            |   Subject   |
            +-------------+
           /      |       \
          ↓       ↓        ↓
  +--------+  +--------+  +--------+
  |Observer|  |Observer|  |Observer|
  +--------+  +--------+  +--------+
```

# Java Example

We'll implement a **Weather Station** system where observers (e.g., displays) react to temperature updates.

```java
// Step 1: Observer interface
interface Observer {
    void update(float temperature);
}
```

```java
// Step 2: Subject interface
interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```

```java
// Step 3: Concrete subject
import java.util.ArrayList;
import java.util.List;

class WeatherStation implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private float temperature;

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers() {
        for (Observer o : observers) {
            o.update(temperature);
        }
    }

    public void setTemperature(float temperature) {
        this.temperature = temperature;
        notifyObservers();
    }
}
```

```java
// Step 4: Concrete observers
class PhoneDisplay implements Observer {
    private String name;

    public PhoneDisplay(String name) {
        this.name = name;
    }

    public void update(float temperature) {
        System.out.println(name + " received temperature update: " + temperature + "°C");
    }
}

class DesktopDisplay implements Observer {
    public void update(float temperature) {
        System.out.println("Desktop display shows: " + temperature + "°C");
    }
}
```

```java
// Step 5: Client
public class ObserverPatternDemo {
    public static void main(String[] args) {
        WeatherStation station = new WeatherStation();

        Observer phone = new PhoneDisplay("Phone A");
        Observer desktop = new DesktopDisplay();

        station.registerObserver(phone);
        station.registerObserver(desktop);

        station.setTemperature(25.5f); // All observers notified
        station.setTemperature(30.0f);

        station.removeObserver(phone);
        station.setTemperature(28.0f); // Only desktop receives this update
    }
}
```

## Output:

```
Phone A received temperature update: 25.5°C
Desktop display shows: 25.5°C
Phone A received temperature update: 30.0°C
Desktop display shows: 30.0°C
Desktop display shows: 28.0°C
```

# Benefits

- **Loose coupling**: The subject doesn't need to know anything about the observers.
- **Dynamic relationships**: Observers can be added or removed at runtime.
- **Scalability**: Easy to add new observers without modifying existing subject code.

# Drawbacks

- **Notification overhead**: If there are many observers or frequent updates, performance can degrade.
- **Order of notification**: Can be unpredictable unless explicitly handled.
- **Potential memory leaks**: If observers are not removed properly.

# Common Uses

- GUI frameworks (button click listeners, form updates)
- Model-View-Controller (MVC) architectures
- Event-driven programming
- Distributed systems (publish/subscribe models)
- Messaging frameworks

# Java Built-in Support

Java has built-in support with `java.util.Observer` and `Observable`, though they're now deprecated in favor of using custom implementations or reactive frameworks like:

- **JavaBeans PropertyChangeSupport**
- **RxJava** (Reactive Extensions for Java)
- **EventBus**, **Reactor**, etc.