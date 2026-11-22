---
tags:
  - FORMAT
  - JavaScript
---
# Object Shorthand and Computed Properties in JavaScript

JavaScript provides some powerful features for working with objects, including **object shorthand** and **computed properties**. These features simplify object creation, improve code readability, and add flexibility to how properties are defined.

---

## 1️⃣ Object Shorthand

**Object shorthand** allows you to create objects with properties that have the same name as the variables assigned to them. This reduces repetition and makes the code more concise.

### Syntax

If the variable name matches the property name, you can omit the value and just write the key name.

```javascript
const key = "name";
const value = "Alice";

const obj = { key, value };
console.log(obj); // Output: { key: "name", value: "Alice" }
```

### Example with Variables

```javascript
let name = "Alice";
let age = 25;

const person = { name, age };
console.log(person); // Output: { name: "Alice", age: 25 }
```

In this example, the object `person` is created with properties `name` and `age` that have the same names as the variables `name` and `age`. Instead of writing `name: name` and `age: age`, you can simply use the shorthand `{ name, age }`.

---

## 2️⃣ Object Shorthand with Functions

If a function is a method of an object, you can use the shorthand for defining methods too.

### Syntax

```javascript
const obj = {
  method() {
    // Function body
  }
};
```

### Example

```javascript
const person = {
  name: "Alice",
  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

person.greet(); // Output: Hello, Alice
```

Here, `greet()` is a method in the `person` object. Using the shorthand, the method is written without the `function` keyword.

---

## 3️⃣ Computed Properties

Computed properties in JavaScript allow you to define object property names dynamically, using expressions. This gives you the ability to use variables or expressions as the property names, rather than just static strings.

### Syntax

You can use square brackets (`[]`) to define a computed property name in an object.

```javascript
const obj = {
  [expression]: value
};
```

### Example with Computed Properties

```javascript
let dynamicKey = "age";
const person = {
  name: "Alice",
  [dynamicKey]: 25
};

console.log(person); // Output: { name: "Alice", age: 25 }
```

In this example, the property name `age` is dynamically set using the value of the variable `dynamicKey`.

---

## 4️⃣ Computed Properties with Expressions

Computed property names can also be expressions. You can use operations, functions, or any valid JavaScript expression to calculate the property name.

### Example with Expression

```javascript
const obj = {
  ["name" + "Key"]: "Alice"
};

console.log(obj); // Output: { nameKey: "Alice" }
```

Here, the property name `nameKey` is created by concatenating the string `"name"` and `"Key"`. The computed property is evaluated as `nameKey`.

### Example with Dynamic Computed Property

```javascript
let year = 2022;
const obj = {
  [`key_${year}`]: "some value"
};

console.log(obj); // Output: { key_2022: "some value" }
```

In this example, the property name `key_2022` is created using the value of the `year` variable.

---

## 5️⃣ Combining Object Shorthand and Computed Properties

You can use both **object shorthand** and **computed properties** together to create even more flexible and concise object definitions.

### Example Combining Shorthand and Computed Properties

```javascript
let key = "age";
let value = 30;

const person = {
  name: "Alice",
  [key]: value
};

console.log(person); // Output: { name: "Alice", age: 30 }
```

In this example:

* The property `name` is assigned using **object shorthand** (since `name` is a variable with the same name).

* The property `age` is created dynamically using **computed properties**.

---

## 6️⃣ Practical Use Cases for Computed Properties

* **Dynamic property names**: When you want to define property names based on user input or program logic.

* **Accessing deeply nested properties**: When working with dynamic keys in deeply nested structures.

* **Creating objects from arrays or other structures**: Dynamically generating keys from arrays or other sources.

### Example of Dynamic Property Creation

```javascript
let prefix = "user_";
let id = 123;

const user = {
  [`${prefix}${id}`]: "Active"
};

console.log(user); // Output: { user_123: "Active" }
```

In this case, the property name is dynamically created by combining the `prefix` and `id` variables.

---

## 7️⃣ Summary

* **Object Shorthand**: When the property name and variable name are the same, you can omit the `key: value` syntax and just write `{ key }`.

* **Computed Properties**: You can use expressions or variables as property names by wrapping them in square brackets `[]`. This allows you to define property names dynamically.

* **Combining Both**: You can use both shorthand and computed properties together to create more flexible and readable objects.

These features streamline how you work with objects in JavaScript, making your code more concise and maintainable, especially when dealing with dynamic keys or method definitions.

