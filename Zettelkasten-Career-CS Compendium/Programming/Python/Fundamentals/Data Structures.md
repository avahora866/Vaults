---
tags:
  - Python
  - FORMAT
---
# Lists

A **list** is an ordered, mutable collection that can hold elements of different types.

## Example:

```py
 Creating a list
fruits = ["apple", "banana", "cherry"]

 Adding an element
fruits.append("orange")

 Removing an element
fruits.remove("banana")

 Accessing elements
irst_fruit = fruits[0]  # "apple"

 Iterating through a list
for fruit in fruits:
    print(fruit)

 Output: apple, cherry, orange
```

---

# Tuples

A **tuple** is an ordered, immutable collection that can hold multiple data types.

## Example:

```py
 Creating a tuple
coordinates = (10, 20)

 Accessing elements
 = coordinates[0]  # 10

 Unpacking
, y = coordinates  # x=10, y=20

 Tuples are immutable, so this will cause an error:
 coordinates[0] = 15  # TypeError: 'tuple' object does not support item assignment
```

---

# Dictionaries

A **dictionary** is an unordered, mutable collection that stores key-value pairs.

## Example:

```py
 Creating a dictionary
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

 Accessing values
ame = person["name"]  # "Alice"

 Adding a new key-value pair
person["job"] = "Engineer"

 Removing a key-value pair
del person["age"]

 Iterating through dictionary keys and values
for key, value in person.items():
    print(f"{key}: {value}")

 Output: 
 name: Alice
 city: New York
 job: Engineer
```

---

# Sets

A **set** is an unordered collection of unique elements.

## Example:

```py
 Creating a set
unique_numbers = {1, 2, 3, 4, 5}

 Adding an element
unique_numbers.add(6)

 Removing an element
unique_numbers.discard(3)

 Checking for existence
rint(4 in unique_numbers)  # True

 Set operations
set_a = {1, 2, 3}
set_b = {3, 4, 5}

 Union
rint(set_a | set_b)  # {1, 2, 3, 4, 5}

 Intersection
rint(set_a & set_b)  # {3}

 Difference
rint(set_a - set_b)  # {1, 2}
```

---

# List & Dictionary Comprehensions

List and dictionary comprehensions provide a concise way to create lists and dictionaries.

## List Comprehension:

```py
 Squaring numbers using list comprehension
squares = [x ** 2 for x in range(1, 6)]
rint(squares)  # [1, 4, 9, 16, 25]
```

## Dictionary Comprehension:

```py
 Creating a dictionary with numbers and their squares
squares_dict = {x: x ** 2 for x in range(1, 6)}
rint(squares_dict)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

---

# Slicing and Indexing

Slicing and indexing allow efficient data access in sequences like lists, tuples, and strings.

## Indexing:

```py
 Accessing elements
nums = [10, 20, 30, 40, 50]
rint(nums[0])   # 10
rint(nums[-1])  # 50 (last element)
```

## Slicing:

```py
 Extracting a subset
nums = [10, 20, 30, 40, 50]

rint(nums[1:4])   # [20, 30, 40] (elements from index 1 to 3)
rint(nums[:3])    # [10, 20, 30] (first three elements)
rint(nums[2:])    # [30, 40, 50] (elements from index 2 to end)
rint(nums[::-1])  # [50, 40, 30, 20, 10] (reversed list)
```

Each of these data structures and techniques is useful in different scenarios, helping to efficiently store and manipulate data in Python.

