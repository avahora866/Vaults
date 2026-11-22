---
tags:
  - DesignPattern
---
Design patterns are elegant solutions to repeating problems in software design.

Mainly based on the book Design Patterns - Elements of Reusable Object-Oriented Software. This book breaks the design patterns into 3 categories:

1. **Creational**: Different ways to create objects
2. **Structural**: Relationships between Objects
3. **Behavioural**: Interaction or communication between objects
4. **JEE Design patterns**: Concerned with providing solutions to the Java EE based software applications and frameworks

- Note: These do not represent all the design patterns, but are the most common used

# Creational design patterns

[[Factory Pattern|Factory design pattern]]: Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

[[Abstract Factory Pattern|Abstract Factory pattern]]: Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

[[Singleton Pattern|Singleton Pattern]]: Singleton is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

[[Prototype Pattern|Prototype Pattern]]: Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

[[Builder Pattern|Builder Pattern]]: Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

# Structural Design Patterns
[[Facade Pattern]]: creating an interface, a gateway to a complex system

[[Bridge Pattern]]: used to bridge between two relating dimensions colour and shape

[[Composite Pattern]]: Composite is a structural design pattern that lets you compose objects into tree structures and then work with these structures as if they were individual objects.

[[Decorator Pattern]]: Decorator is a structural design pattern that lets you attach new behaviours to objects by placing these objects inside special wrapper objects that contain the behaviours.

[[Adapter Pattern]]: Adaptor is a structural design pattern that allows objects with incompatible interfaces to collaborate.

[[Flyweight Pattern]]: extrinsic fields, being fields that change object to object. Intrinsic data is constant between object. Making an object only hold intrinsic values would lead to an optimization in RAM usage

[[Proxy Pattern]]: Proxy is a structural design pattern that lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

# Behavioural Design Pattern

[[Strategy Pattern]]: Strategy is a behavioural design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

[[Iterator Pattern]]: Iterator is a behavioural design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

 [[Mediator Pattern]]: Mediator is a behavioural design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

[[Command Pattern]]: Command is a behavioural design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as method arguments, delay or queue a request’s execution, and support undoable operations.

[[State Pattern]]: State is a behavioural design pattern that lets an object alter its behaviour when its internal state changes. It appears as if the object changed its class.

[[Observer Pattern]]: Observer is a behavioural design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

[[Template Method Pattern]]: Template Method is a behavioural design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

[[Visitor Pattern]]: Visitor is a behavioural design pattern that lets you separate algorithms from the objects on which they operate.

[[Chain of Responsibility Pattern]]: Chain of Responsibility is a behavioural design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.
