---
tags:
  - FORMAT
  - JavaScript
---
# Object Methods and Properties in JavaScript

## 1️⃣ What are Object Properties?

**Properties** are key-value pairs that store **data** inside an object.

### Example: Defining Properties

```javascript
const person = {
  name: "Alice",
  age: 25,
  city: "New York"
};

console.log(person.name); // Output: Alice
console.log(person["age"]); // Output: 25
```

🔹 Properties can be accessed using **dot notation (`.`)** or **bracket notation (`[]`)**.  
 🔹 Bracket notation is useful when property names **contain spaces** or **are dynamic**.

---

## 2️⃣ What are Object Methods?

**Methods** are functions inside an object that define behavior.

### Example: Defining Methods

```javascript
const person = {
  name: "Alice",
  greet: function() {
    console.log(`Hello, my name is ${this.name}.`);
  }
};

person.greet(); // Output: Hello, my name is Alice.
```

🔹 The `this` keyword refers to the object that calls the method.  
 🔹 Methods are **functions stored as object properties**.

---

## 3️⃣ Adding & Modifying Properties and Methods

### Adding New Properties

```javascript
const car = {
  brand: "Toyota"
};

car.model = "Camry"; // Adding a new property
console.log(car); // Output: { brand: "Toyota", model: "Camry" }
```

### Modifying Properties

```javascript
car.brand = "Honda"; // Modifying existing property
console.log(car.brand); // Output: Honda
```

### Adding Methods

```javascript
car.getInfo = function() {
  return `${this.brand} ${this.model}`;
};

console.log(car.getInfo()); // Output: Honda Camry
```

---

## 4️⃣ Deleting Properties and Methods

Use the `delete` operator to remove a property or method.

```javascript
delete car.model;
console.log(car); // Output: { brand: "Honda", getInfo: [Function] }

delete car.getInfo;
console.log(car); // Output: { brand: "Honda" }
```

---

## 5️⃣ Checking if a Property Exists

### Using `in` Operator

```javascript
console.log("brand" in car); // Output: true
console.log("model" in car); // Output: false
```

### Using `hasOwnProperty()`

```javascript
console.log(car.hasOwnProperty("brand")); // Output: true
console.log(car.hasOwnProperty("model")); // Output: false
```

---

## 6️⃣ Looping Through Object Properties

Use **`for...in`** to iterate over an object’s properties.

```javascript
const user = { name: "Bob", age: 30, city: "Paris" };

for (let key in user) {
  console.log(`${key}: ${user[key]}`);
}
```

### Output:

```
name: Bob
age: 30
city: Paris
```

---

## 7️⃣ Common Built-in Object Methods

| Method | Description | Example |
| ----- | ----- | ----- |
| `Object.keys(obj)` | Returns an array of keys | `Object.keys(user)` → `["name", "age", "city"]` |
| `Object.values(obj)` | Returns an array of values | `Object.values(user)` → `["Bob", 30, "Paris"]` |
| `Object.entries(obj)` | Returns an array of key-value pairs | `Object.entries(user)` → `[["name", "Bob"], ["age", 30], ["city", "Paris"]]` |
| `Object.assign(target, source)` | Copies properties from `source` to `target` | `Object.assign({}, user)` |
| `Object.freeze(obj)` | Prevents modifications to the object | `Object.freeze(user)` |
| `Object.seal(obj)` | Prevents adding/removing properties but allows modification | `Object.seal(user)` |

---

## 🚀 Summary

✔ **Properties** store data in key-value pairs.  
 ✔ **Methods** are functions stored inside an object.  
 ✔ **Modify, delete, and check properties dynamically.**  
 ✔ **Use loops and built-in methods (`keys()`, `values()`, `entries()`) to manipulate objects efficiently.**

Mastering object properties and methods is essential for **working with JavaScript objects effectively\!** 🚀

