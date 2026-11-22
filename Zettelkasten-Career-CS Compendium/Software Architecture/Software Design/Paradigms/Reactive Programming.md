---
tags:
  - Paradigms
  - "#TRUNK"
---
**Reactive programming** is a programming paradigm for working with asynchronous data streams and event-driven systems. It’s designed to help build systems that are responsive, resilient, and scalable.

---

# Core Concept

**Data as a stream**:
Reactive programming treats values and events as continuous streams over time.

* You subscribe to these streams.
* When new data arrives, the system pushes it to you.
* Your code reacts to updates, errors, or stream completion.

This differs from imperative programming, where you pull or poll for data.

---

# Key Principles

| Principle       | Description                                                               |
| --------------- | ------------------------------------------------------------------------- |
| Asynchronous    | Work with events and data that arrive over time without blocking threads. |
| Non-blocking    | Avoid waiting; use callbacks or event handlers to respond to changes.     |
| Event-driven    | Code reacts to events instead of executing linearly.                      |
| Stream-oriented | Data is represented as sequences (streams) of values or events over time. |
| Backpressure    | Controls data flow between producers and consumers to prevent overload.   |
| Declarative     | You describe *what* to do with the data, not *how* to do it.              |
| Compositional   | Operators can combine, transform, or filter streams functionally.         |

---

# Common Use Cases

* User interfaces (reacting to user input)
* Real-time data processing (e.g., sensor data, stock feeds)
* Network or I/O heavy applications
* Reactive web APIs and microservices

---

# Example in Java (using RxJava)

```java
Observable<String> source = Observable.just("Alpha", "Beta", "Gamma");

source
    .map(String::toUpperCase)
    .filter(s -> s.length() > 4)
    .subscribe(System.out::println);
```

This:

* Creates a stream of strings
* Maps each to uppercase
* Filters strings with length > 4
* Subscribes and prints them

---

# Popular Libraries and Frameworks

* **Java**: RxJava, Project Reactor (used in Spring WebFlux)
* **JavaScript/TypeScript**: RxJS (used with Angular)
* **.NET**: Reactive Extensions (Rx.NET)
* **Kotlin**: Flow, Coroutine-based reactive streams

---

# Reactive vs. Imperative

| Feature        | Imperative                | Reactive                    |
| -------------- | ------------------------- | --------------------------- |
| Flow control   | Synchronous, blocking     | Asynchronous, non-blocking  |
| Structure      | Step-by-step instructions | Data flow and event-based   |
| Responsiveness | May block on I/O          | Designed for responsiveness |

---

# Related Concepts

* **Observer pattern**: Reactive builds on this pattern; observers subscribe to observable subjects.
* **Functional programming**: Emphasizes pure functions and immutability; pairs well with reactive streams.
* **Reactive systems**: Broader architectural style encompassing reactive programming, event sourcing, etc.

---
