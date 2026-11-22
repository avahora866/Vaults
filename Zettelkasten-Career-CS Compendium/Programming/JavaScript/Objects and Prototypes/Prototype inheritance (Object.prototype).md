---
tags:
  - FORMAT
  - JavaScript
---
# Prototype Inheritance in JavaScript

In JavaScript, **inheritance** works through prototypes, which allows objects to share properties and methods. This enables objects to inherit properties and methods from other objects, creating a powerful mechanism for reusability and code efficiency.

---

## 1️⃣ What is Prototype Inheritance?

Every JavaScript object has an internal property called `[[Prototype]]` (accessed via `__proto__` or `Object.getPrototypeOf()`) that points to another object. This object is called the **prototype**. Through **prototype inheritance**, an object can inherit properties and methods from its prototype.

### Example: Object Prototype Chain

```javascript
const animal = {
  eats: true,
  sound() {
    console.log("Animal sound");
  }
};

const dog = Object.create(animal);
dog.bark = function() {
  console.log("Woof!");
};

dog.bark(); // Output: Woof!
dog.sound(); // Output: Animal sound (inherited from animal prototype)
```

🔹 Here, `dog` inherits from the `animal` object.  
 🔹 **`dog`** can access properties and methods of its **prototype** (the `animal` object) like `sound()`.

---

## 2️⃣ Object.prototype

At the top of the prototype chain is **`Object.prototype`**, the prototype object of all JavaScript objects (except for `null`). It contains built-in methods like `toString()`, `hasOwnProperty()`, and more, that are inherited by all objects.

### Example: Accessing `Object.prototype` Methods

```javascript
const person = {
  name: "Alice",
  age: 25
};

console.log(person.hasOwnProperty("name")); // Output: true
console.log(person.toString()); // Output: [object Object]
```

🔹 **`hasOwnProperty()`** and **`toString()`** are inherited from `Object.prototype`, allowing all objects to access these methods.

---

## 3️⃣ Prototype Chain

In JavaScript, the **prototype chain** is a series of objects linked together. When a property is accessed on an object, JavaScript looks for it in the object itself. If it doesn't find it, it checks the object's **prototype**. This process continues up the prototype chain until the property is found or the chain ends (usually at `Object.prototype`).

### Example: Exploring the Prototype Chain

```javascript
const person = {
  name: "Bob",
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

const student = Object.create(person);
student.major = "Computer Science";

student.greet(); // Output: Hello, my name is Bob (inherited from person)
console.log(student.major); // Output: Computer Science
```

🔹 Here, `student` inherits `greet()` from `person`, but also has its own `major` property.  
 🔹 The **prototype chain** starts with `student` → `person` → `Object.prototype`.

---

## 4️⃣ How to Work with Prototypes

### Using `Object.create()`

`Object.create()` creates a new object and sets its prototype to the object you pass as an argument.

```javascript
const animal = {
  eats: true,
  sound() {
    console.log("Animal sound");
  }
};

const dog = Object.create(animal); // `dog` inherits from `animal`
dog.bark = function() {
  console.log("Woof!");
};

dog.bark(); // Output: Woof!
dog.sound(); // Output: Animal sound
```

### Accessing and Modifying Prototypes

You can use **`Object.getPrototypeOf()`** to get the prototype of an object and **`Object.setPrototypeOf()`** to modify the prototype.

```javascript
const person = {
  name: "Alice"
};

const student = {
  major: "Math"
};

Object.setPrototypeOf(student, person); // Set `person` as prototype of `student`

console.log(student.name); // Output: Alice (inherited from person)
console.log(Object.getPrototypeOf(student)); // Output: { name: "Alice" }
```

### Adding Methods to Prototypes

You can add methods to an object's prototype so that all instances of that object can access those methods.

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Adding a method to the prototype
Person.prototype.greet = function() {
  console.log(`Hello, my name is ${this.name}`);
};

const person1 = new Person("Bob", 30);
const person2 = new Person("Alice", 25);

person1.greet(); // Output: Hello, my name is Bob
person2.greet(); // Output: Hello, my name is Alice
```

🔹 Both `person1` and `person2` can access the `greet()` method, even though it was defined on `Person.prototype`.

---

## 5️⃣ Prototype Inheritance with Constructor Functions

In JavaScript, when you create an object with a **constructor function**, the object inherits from the constructor function's prototype.

### Example: Constructor Function Inheritance

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  console.log(`${this.name} makes a noise.`);
};

function Dog(name, breed) {
  Animal.call(this, name); // Inherit properties from Animal
  this.breed = breed;
}

// Set Dog's prototype to be an instance of Animal
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  console.log(`${this.name} barks.`);
};

const dog1 = new Dog("Rex", "German Shepherd");

dog1.speak(); // Output: Rex makes a noise.
dog1.bark(); // Output: Rex barks.
```

🔹 `Dog` inherits from `Animal` via **`Animal.call(this, name)`** and **`Dog.prototype = Object.create(Animal.prototype)`**.  
 🔹 The `Dog` object can access methods from both its own prototype and the `Animal` prototype.

---

## 6️⃣ Prototype Chain and Performance Considerations

### Memory Efficiency

* Methods defined on an object's prototype are shared by all instances, making them more **memory-efficient** compared to defining methods inside the constructor function (where methods are created anew for each instance).

### Performance

* Accessing properties and methods via the prototype chain may have some performance overhead, as the JavaScript engine needs to traverse the chain. However, this overhead is usually negligible for most applications.

---

## 7️⃣ Summary of Prototype Inheritance

| Feature | Description |
| ----- | ----- |
| **Prototype Chain** | Objects inherit properties and methods from their prototype chain, starting from the object itself. |
| **`Object.prototype`** | The topmost prototype in all JavaScript objects. Inherited methods like `toString()` and `hasOwnProperty()` are part of `Object.prototype`. |
| **Creating Prototypes** | Use `Object.create()` to set an object’s prototype, or use constructor functions and `class` to create objects with inherited methods. |
| **Performance** | Using prototype methods is memory-efficient because all instances share the same methods defined on the prototype. |

### ✅ Key Takeaways:

* **Prototype inheritance** is the mechanism through which objects can inherit properties and methods from other objects.

* **`Object.prototype`** is the root of the prototype chain, providing shared methods to all objects.

* **`Object.create()`** and constructor functions allow you to define inheritance in JavaScript.

By mastering prototype inheritance, you can write more efficient and reusable code in JavaScript\! 🚀

