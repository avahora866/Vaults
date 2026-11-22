---
tags:
  - FORMAT
  - Java
---
Certainly\! Here's the same content reformatted into a more narrative, detailed note, minimizing bullet points and using clear paragraphs and sections instead of bullet lists.

---

# Java `String` Data Structure — Deep Overview

## Introduction

In Java, the `String` class represents an immutable sequence of characters and is one of the most fundamental and widely used classes in the language. Strings are used everywhere—from basic data output to keys in hash maps—and their behavior has been finely tuned for performance, safety, and usability.

## Internal Representation

The internal representation of a `String` has evolved over time. In Java 8 and earlier, a `String` stores its characters in a `char[]` array. This array is marked `final`, meaning it cannot be reassigned after the string is constructed. Because the array is private and the string is immutable, the characters it holds cannot be changed.

Starting from Java 9, Java introduced a more memory-efficient representation called **Compact Strings**. Instead of `char[]`, Java uses a `byte[]` to store character data, along with a `coder` flag that indicates whether the content is encoded in Latin-1 (1 byte per character) or UTF-16 (2 bytes per character). This change drastically reduces memory usage for applications dealing primarily with ASCII or Western European text.

Despite the internal shift, the external API remains consistent: developers continue to interact with `String` as a sequence of characters, regardless of whether it is backed by `char[]` or `byte[]`.

## Immutability

One of the most important properties of the `String` class is its immutability. Once a `String` object is created, its value cannot be altered. Operations that appear to modify a string—such as `concat()`, `substring()`, `replace()`, or `toUpperCase()`—always return a new `String` object rather than changing the original.

Immutability provides several benefits. It makes strings inherently thread-safe, since multiple threads can share references to the same `String` without risk of interference. It also enables strings to be cached and reused safely, which brings us to the next major concept: the string pool.

## The String Pool

The Java Virtual Machine maintains a special memory area called the **string pool** or **intern pool**. This pool contains unique references to string literals and explicitly interned strings. When a string literal such as `"hello"` is encountered in code, the JVM checks if that string already exists in the pool. If it does, the existing reference is reused. If it doesn't, the string is added to the pool.

This behavior means that two string literals with the same contents will actually reference the same object in memory. For example:

```java
String a = "hello";
String b = "hello";
System.out.println(a == b); // true
```

However, if a string is created explicitly with the `new` keyword—e.g., `new String("hello")`—a new object is allocated on the heap, bypassing the pool. If you want to place it in the pool, you must call `.intern()`, which returns the canonical pooled instance.

## Equality and Comparison

In Java, using `==` to compare objects checks whether the two references point to the exact same object. For strings, this only returns `true` if both references point to the same instance. Because of this, `.equals()` must be used for content comparison. The `String` class overrides `Object.equals()` to compare the contents of the string—character by character.

So:

```java
String a = "test";
String b = new String("test");

a == b         // false
a.equals(b)    // true
```

The `.equalsIgnoreCase()` method is also available to compare strings without considering letter case.

## Hashing and HashMap Compatibility

The `String` class also overrides `hashCode()`, which computes a hash based on the string’s content. The hashing algorithm is simple but effective: it processes each character from left to right and multiplies the current hash by 31 before adding the next character's value. This results in a relatively well-distributed hash value that is cached after the first computation to optimize performance during repeated hash lookups (e.g., in a `HashMap`).

Because strings are immutable and have a stable hash code, they are ideal for use as keys in hash-based collections like `HashMap` or `HashSet`.

## Common Operations

The `String` class offers a rich API for manipulation and querying. You can retrieve the length of a string with `length()`, access a specific character with `charAt(int index)`, and extract substrings with `substring()`. Concatenation is supported through both the `+` operator (which the compiler translates into `StringBuilder` operations) and the `concat()` method.

Other useful methods include `contains()`, `indexOf()`, `startsWith()`, `endsWith()`, and `split()` for regex-based tokenization. Strings can also be converted into arrays of characters using `toCharArray()`.

Because strings are immutable, any operation that transforms a string returns a new `String` instance, not a modified version of the original.

## Thread Safety

Since `String` is immutable, it is naturally thread-safe. Multiple threads can safely share the same `String` instance without any need for synchronization. This is one reason why string literals and pooled strings can be widely reused across a program without any concern about thread interference.

## Implementation Constraints

The `String` class is declared `final`, meaning it cannot be subclassed. This prevents extension or modification of string behavior and helps preserve the immutability and integrity of string operations. Additionally, `String` implements the `Comparable<String>` interface, allowing for lexicographic ordering using `compareTo()`.

## Memory Optimization in Java 9+

With Java 9’s Compact Strings feature, Java stores Latin-1-compatible strings using a single byte per character. A `coder` flag indicates whether the `byte[]` array should be interpreted as Latin-1 or UTF-16. This optimization is internal and transparent to the developer, but it allows substantial memory savings, especially in applications dealing with primarily English or numeric content.

## Summary

The Java `String` class is a robust, immutable data structure built for efficiency and safety. Under the hood, it uses a character or byte array to store content, depending on the Java version. It supports the string pool mechanism for memory sharing, overrides `equals()` and `hashCode()` for logical equality and hashing, and exposes a rich set of methods for manipulation. Its design enables reliable use in multi-threaded environments, makes it ideal for use in maps and sets, and helps Java developers write safe and expressive code.

---

Let me know if you'd like a companion note for `StringBuilder`, `StringBuffer`, or comparisons among them\!

