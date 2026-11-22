---
tags:
  - Java
---
Any group of individual objects that are represented as a single unit is known as a Java Collection of Objects. In Java, a separate framework named the “Collection Framework” has been defined in JDK 1.2 which holds all the Java Collection Classes and Interface in it.

In Java, the Collection interface (java.util.Collection) and Map interface (java.util.Map) are the two main “root” interfaces of Java collection classes.

Before the Collection Framework(or before JDK 1.2) was introduced, the standard methods for grouping Java objects (or collections) were Arrays or Vectors, or Hashtables. All of these collections had no common interface. Therefore, though the main aim of all the collections is the same, the implementation of all these collections was defined independently and had no correlation among them. And also, it is very difficult for the users to remember all the different methods, syntax, and constructors present in every collection class.

# Advantages of the Java Collection Framework

Since the lack of a collection framework gave rise to the above set of disadvantages, the following are the advantages of the collection framework.

- Consistent API: The API has a basic set of interfaces like Collection, Set, List, or Map, all the classes (ArrayList, LinkedList, Vector, etc) that implement these interfaces have some common set of methods.
- Reduces programming effort: A programmer doesn’t have to worry about the design of the Collection but rather he can focus on its best use in his program. Therefore, the basic concept of Object-oriented programming (i.e.) abstraction has been successfully implemented.
- Increases program speed and quality: Increases performance by providing high-performance implementations of useful data structures and algorithms because in this case, the programmer need not think of the best implementation of a specific data structure. He can simply use the best implementation to drastically boost the performance of his algorithm/program.

# Hierarchy of the Collection Framework in Java

The utility package, (`java.util`) contains all the classes and interfaces that are required by the collection framework. The collection framework contains an interface named an Iterable interface which provides the [[Iterator Pattern|iterator]] to iterate through all the collections. This interface is extended by the main collection interface which acts as a root for the collection framework. All the collections extend this collection interface thereby extending the properties of the [[Iterator Pattern|iterator]] and the methods of this interface. The following figure illustrates the hierarchy of the collection framework.

![Collections-in-Java.png](Collections-in-Java.png)

# Interfaces that extend the java collections interface

The collection framework contains multiple interfaces where every interface is used to store a specific type of data. The following are the interfaces present in the framework.

## Iterable Interface

This is the root interface for the entire collection framework. The collection interface extends the Iterable interface. Therefore, inherently, all the interfaces and classes implement this interface. The main functionality of this interface is to provide an [[Iterator Pattern|iterator]] for the collections. Therefore, this interface contains only one abstract method which is the [[Iterator Pattern|iterator]]. It returns the

## Collection Interface

This interface extends the iterable interface and is implemented by all the classes in the collection framework. This interface contains all the basic methods which every collection has like adding the data into the collection, removing the data, clearing the data, etc. All these methods are implemented in this interface because these methods are implemented by all the classes irrespective of their style of implementation. And also, having these methods in this interface ensures that the names of the methods are universal for all the collections. Therefore, in short, we can say that this interface builds a foundation on which the collection classes are implemented.

## List Interface

This is a child interface of the collection interface. This interface is dedicated to the data of the list type in which we can store all the ordered collections of the objects. This also allows duplicate data to be present in it. This list interface is implemented by various classes like ArrayList, Vector, Stack, etc. Since all the subclasses implement the list, we can instantiate a list object with any of these classes.

### Classes which implement the List interface:

- ArrayList
- LinkedList
- Vector
- Stack

## Queue Interface

As the name suggests, a queue interface maintains the FIFO(First In First Out) order similar to a real-world queue line. This interface is dedicated to storing all the elements where the order of the elements matter. For example, whenever we try to book a ticket, the tickets are sold on a first come first serve basis. Therefore, the person whose request arrives first into the queue gets the ticket. There are various classes like PriorityQueue, ArrayDeque, etc. Since all these subclasses implement the queue, we can instantiate a queue object with any of these classes.

**The most frequently used implementation of the queue interface is the PriorityQueue.**

## Deque Interface

This is a very slight variation of the queue data structure. Deque, also known as a double-ended queue, is a data structure where we can add and remove elements from both ends of the queue. This interface extends the queue interface. The class which implements this interface is ArrayDeque. Since ArrayDeque class implements the Deque interface, we can instantiate a deque object with this class.

****The class which implements the deque interface is ArrayDeque.****

## Set Interface

A set is an unordered collection of objects in which duplicate values cannot be stored. This collection is used when we wish to avoid the duplication of the objects and wish to store only the unique objects. This set interface is implemented by various classes like HashSet, TreeSet, LinkedHashSet, etc. Since all the subclasses implement the set, we can instantiate a set object with any of these classes.

### The following are the classes that implement the Set interface:

- HashSet
- LinkedHashSet

## Sorted Set Interface

This interface is very similar to the set interface. The only difference is that this interface has extra methods that maintain the ordering of the elements. The sorted set interface extends the set interface and is used to handle the data which needs to be sorted. The class which implements this interface is TreeSet. Since this class implements the SortedSet, we can instantiate a SortedSet object with this class.

#### The class which implements the sorted set interface is TreeSet.

## Map Interface

A map is a data structure that supports the key-value pair for mapping the data. This interface doesn’t support duplicate keys because the same key cannot have multiple mappings, however, it allows duplicate values in different keys. A map is useful if there is data and we wish to perform operations on the basis of the key. This map interface is implemented by various classes like HashMap, TreeMap, etc. Since all the subclasses implement the map, we can instantiate a map object with any of these classes.

****The frequently used implementation of a Map interface is a HashMap.****