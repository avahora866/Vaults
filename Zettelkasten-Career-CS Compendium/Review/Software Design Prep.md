- [[_Object Oriented Programming (OOP)]]
- [[1. What is Solid]]
- - Has a and Is a: Inheritance vs Composition
- Design Patterns
	- [[Singleton Pattern]]
		- `private static singleton` field inside singleton class
		- `get Method` that returns filed and if not instantiated instantiates it
	- [[Factory Pattern]] and [[Abstract Factory Pattern]]
		- factory pattern
			- Interface and implementations. Concrete class that has a `get Interface` method that takes in a string of type which decides which implementation to return
		- abstract factory
			- Same concept as above but now we are instantiating multiple implementations of interface or interfaces. The implementations are related (windows, mac). The abstract factory has methods to return the interface not implementations. The abstract factory implementations override the method and return the implementation they need
	- [[Builder Pattern]]
		- Concrete class which we are building. Builder interface has concrete class as a field which is instantiated through the builder implementation constructor. Client code calls builder methods to construct the concrete class. Multiple builder implementations allows different configurations of the concrete class
	- [[Strategy Pattern]]
		- Switching algorithms around. `StrategyInterface` is implements by the algorithms. The context class has the `StrategyInterface` as a field inside it's class and calls the algorithm implementation. We can now set whatever algorithm we want from the set method inside the context class
	- [[Decorator Pattern]] 
		- Adds more functionality to a simple base implementation by adding decorators. 
		- Interface implemented by simple base implementation and decorator class. Decorator class has the interface as a field and is issuing it in the constructed and it's method. Decorator implementations calls the interface methods then adds on its own.
		- By calling the interface methods we still keep the other behaviour from other decorators and base implementation. 
	- [[Proxy Pattern]]
		- Stands in front of an implementation and interface and offers extra behaviour before or after the interface method is called
		- Proxy and actual implementation both implement interface. Proxy has a field of concrete Implementation that is only instantiated when needed
- Efficient Algorithms
- Data Structures 

# How to structure a software design

1. Functional Requirements
2. Core Entities - Dont define them yet, get the basic fields down
3. API Endpoints
4. Controllers, Services, DAOs - here is where you figure out specifically what fields you need for the entities
5. Core entities fields

6. Classes
7. Relationships: Composition vs Inheritance
8. Method
9. Validations and Constraints
	1. **Validations**: Check inputs or state before performing operations.
		1. `Library.checkoutBook(memberId, bookId)` → validate that `memberId` exists, the `bookId` exists, and the book isn’t already checked out.
		2. `User.setEmail(email)` → validate that `email` is in correct format.
	2. **Constraints**: Rules about limits or allowed states 
		1. A `ParkingLot` class: max number of cars = capacity
		2. An `Account` class: balance cannot go below zero
		3. A `Leaderboard` class: only top K scores are stored

# Interview questions
- Design a **Library Management System** (classes for Book, Member, Loan).
- Design a **Parking Lot** class structure (spots, vehicles, tickets).
- Design an **LRU Cache** class with `get` and `put` methods.
- Design a **Bank Account** system (deposit, withdraw, transfer) with constraints.
- Design a **Leaderboard** class (addScore, topK).
- Design a **Messaging/Chat** class (sendMessage, receiveMessage).
- Design a **Restaurant Reservation** system (tables, reservations, availability).
- Design a **File Storage** class structure (files, folders, read/write operations).
- Design a **Movie Ticket Booking** class (seats, shows, booking).
- Design a **Shopping Cart** class (addItem, removeItem, calculateTotal).
- Design a **Social Media Post** class (like, comment, share).
- Design a **Simple Calendar** class (events, reminders, overlaps).
- - Design a **Deck of Cards** (shuffle, draw, deal).