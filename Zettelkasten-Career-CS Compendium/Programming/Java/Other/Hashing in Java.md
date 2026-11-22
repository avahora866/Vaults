---
tags:
  - Java
---
*Hashing* is defined as the process of transforming one value into another based on a particular key. A *hash* is a function that converts an input value into an output value that is usually shorter, and is designed to be unique for each input value. Although collisions are unavoidable, your hash function should attempt to reduce collisions, which implies that different input values should not generate the same hash code.

Hashes are used in many different applications, such as storing passwords, creating unique identifiers, and verifying data. A hash function produces what is known as a *hash value*, a *hash code*, or a hash. A *hash table* is a data structure that stores *key-value* pairs, where each key is used to calculate an index in the table that corresponds to the location of the value.

# Types of Hashing Algorithms in Java

There are several hashing algorithms – the most common ones are: **MD5**, **SHA-1**, and **SHA-256**. These algorithms are used to generate a hash of a given piece of data, which can then be used to verify the integrity of that data.

# What are the Advantages and Disadvantages of Hashing

The main advantage of hashing is that it can be used to store data of any size in a relatively small amount of space. The data is stored in a **“hash table”**, which is a collection of data values that are each assigned a unique key. When you want to retrieve the data, you simply provide the key and the hash table looks up the associated value.

## HashMap and HashSet in Java

Java provides multiple ways to implement hashing. Some of the most popular ways are using the **HashMap** and **HashSet** classes. Both the **HashMap** and **HashSet** classes use hashing algorithms to store and retrieve data.

### HashMap

The **HashMap** class is a part of the *Java Collections Framework*. It stores data represented as key-value pairs where the keys are non-null and unique; for example, duplicate keys are not allowed.

### HashSet

The **HashSet** class is also a part of the Java Collections Framework. It stores data in a set, which means that similar to **HashMap**, it would not allow duplicate values. However, unlike the **HashMap** class, the **HashSet** class does not store data in key-value pairs.

# How to Program Hashing in Java

There are many ways to hash in Java. Some of the most common methods are using the built-in **hashCode** method. To hash a **String** using the built-in **hashCode** method, you can use the following code:

```java
String str = "Hello, world!"; 
int hash = str.hashCode();

```

To hash a **String** using the **SHA-256** hashing algorithm, you can use the following code:

```java
String str = "Hello, world!"; 
String algorithm = "SHA-256"; 
byte[] bytes = Hashing.digest(algorithm, str.getBytes()).asBytes();

```

The following code listing shows how you can generate hash code for variables in Java. Note that the hash code for **str1** and **str2** will differ but the hash code for **str3** and **str4** will be identical:

```java
import java.io.*;
public class Test {
    public static void main(String args[]) {
      String str1 = "Hello";
      String str2 = "World!";
      System.out.println("The hash code of str1 is: " + str1.hashCode());
      System.out.println("nThe hash code of str2 is: " + str2.hashCode());
      String str3 = "Same value";
      String str4 = "Same value";
       System.out.println("The hash code of str3 is: " + str3.hashCode());
      System.out.println("nThe hash code of str4 is: " + str4.hashCode());
   }
}

```

&nbsp;

&nbsp;

&nbsp;