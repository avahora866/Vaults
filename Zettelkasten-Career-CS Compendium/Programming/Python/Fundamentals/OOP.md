---
tags:
  - Python
  - OOP
  - FORMAT
---
# Object-Oriented Programming (OOP) in Python

## Classes and Objects

A **class** is a blueprint for creating objects, and an **object** is an instance of a class.

```py
# Defining a class
class Person:
    def __init__(self, name, age):  # Constructor
        self.name = name
        self.age = age

    def greet(self):
        return f"Hello, my name is {self.name} and I am {self.age} years old."

# Creating an object
p1 = Person("Alice", 25)
print(p1.greet())  # Output: Hello, my name is Alice and I am 25 years old.
```

---

## Inheritance

Inheritance allows a class to inherit methods and attributes from another class.

```py
# Parent class
class Animal:
    def __init__(self, name):
        self.name = name

    def make_sound(self):
        return "Some generic sound"

# Child class (inherits from Animal)
class Dog(Animal):
    def make_sound(self):  # Overriding method
        return "Bark"

dog = Dog("Buddy")
print(dog.name)          # Output: Buddy
print(dog.make_sound())  # Output: Bark
```

### Multiple Inheritance

```py
class Flyable:
    def fly(self):
        return "I can fly!"

class Bird(Animal, Flyable):
    pass

sparrow = Bird("Sparrow")
print(sparrow.name)  # Output: Sparrow
print(sparrow.fly()) # Output: I can fly!
```

---

## Polymorphism

Polymorphism allows different classes to define the same method but with different implementations.

```py
class Cat:
    def make_sound(self):
        return "Meow"

class Dog:
    def make_sound(self):
        return "Bark"

# Using polymorphism
animals = [Cat(), Dog()]

for animal in animals:
    print(animal.make_sound())  # Output: Meow, Bark
```

---

## Encapsulation

Encapsulation restricts direct access to class attributes using **private** (`__`) and **protected** (`_`) variables.

```py
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Private attribute

    def deposit(self, amount):
        self.__balance += amount

    def get_balance(self):
        return self.__balance

# Creating an object
account = BankAccount(1000)
account.deposit(500)
print(account.get_balance())  # Output: 1500

# Trying to access private attribute directly (will cause error)
# print(account.__balance)  # AttributeError
```

---

## Special Methods (`__init__`, `__repr__`, `__call__`)

### `__init__` (Constructor)

Called when an object is created.

```py
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

car = Car("Tesla", "Model S")
print(car.brand, car.model)  # Output: Tesla Model S
```

### `__repr__` (Object Representation)

Returns a string representation of an object.

```py
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

    def __repr__(self):
        return f"Car(brand='{self.brand}', model='{self.model}')"

car = Car("Ford", "Mustang")
print(car)  # Output: Car(brand='Ford', model='Mustang')
```

### `__call__` (Callable Objects)

Makes an object behave like a function.

```py
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, num):
        return num * self.factor

double = Multiplier(2)
print(double(5))  # Output: 10
```

---

## Summary

| Feature | Example |
| ----- | ----- |
| **Class & Object** | `class Person: def greet(self):` |
| **Inheritance** | `class Dog(Animal):` |
| **Polymorphism** | `class Cat: def make_sound(self):` |
| **Encapsulation** | `self.__balance = balance` |
| **`__init__`** | `def __init__(self, name):` |
| **`__repr__`** | `def __repr__(self): return "Object Representation"` |
| **`__call__`** | `def __call__(self, value): return value * 2` |

