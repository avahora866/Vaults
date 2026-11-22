---
tags:
  - FORMAT
  - JavaScript
---
# Destructuring and Spread/Rest Operators in JavaScript

JavaScript provides powerful syntax features like **destructuring** and the **spread/rest operators** to make code cleaner, more readable, and more efficient. Let’s break down these features to understand their usage and benefits.

---

## 1️⃣ Destructuring Assignment

Destructuring allows you to unpack values from arrays or objects and assign them to variables in a very concise and readable way.

### Array Destructuring

Array destructuring allows you to unpack values from arrays into individual variables.

### Syntax

```javascript
const [var1, var2, ...rest] = array;
```

### Example

```javascript
const fruits = ["apple", "banana", "cherry"];
const [firstFruit, secondFruit] = fruits;

console.log(firstFruit); // Output: "apple"
console.log(secondFruit); // Output: "banana"
```

🔹 You can also use **rest syntax** to capture the remaining elements in an array:

```javascript
const [firstFruit, ...otherFruits] = fruits;
console.log(otherFruits); // Output: ["banana", "cherry"]
```

---

### Object Destructuring

Object destructuring allows you to unpack properties from an object into variables.

### Syntax

```javascript
const { key1, key2 } = object;
```

### Example

```javascript
const person = { name: "John", age: 30, country: "USA" };
const { name, age } = person;

console.log(name); // Output: "John"
console.log(age);  // Output: 30
```

🔹 You can also **rename** the variables during destructuring:

```javascript
const { name: fullName, age: personAge } = person;
console.log(fullName); // Output: "John"
console.log(personAge); // Output: 30
```

🔹 If a property is missing from the object, **undefined** will be assigned to the variable. You can provide a **default value** to avoid this:

```javascript
const { name, job = "Unknown" } = person;
console.log(job); // Output: "Unknown"
```

---

### Nested Destructuring

Destructuring can be applied to **nested objects** and **arrays**.

#### Example with Nested Arrays

```javascript
const numbers = [1, [2, 3], 4];
const [one, [two, three], four] = numbers;

console.log(two); // Output: 2
console.log(three); // Output: 3
```

#### Example with Nested Objects

```javascript
const user = {
  name: "Alice",
  address: { city: "New York", zip: "10001" }
};
const { name, address: { city, zip } } = user;

console.log(city); // Output: "New York"
console.log(zip); // Output: "10001"
```

---

## 2️⃣ Spread Operator (`...`)

The **spread operator** allows you to unpack elements from arrays or properties from objects and spread them into another array or object. This is useful when you want to copy or merge arrays or objects.

### Array Spread

The spread operator can be used to copy the elements of an array or to merge arrays.

### Syntax

```javascript
const newArray = [...array];
```

### Example

```javascript
const fruits = ["apple", "banana", "cherry"];
const moreFruits = ["orange", "grape"];
const combinedFruits = [...fruits, ...moreFruits];

console.log(combinedFruits); // Output: ["apple", "banana", "cherry", "orange", "grape"]
```

🔹 **Copying an array**:

```javascript
const original = [1, 2, 3];
const copy = [...original];

console.log(copy); // Output: [1, 2, 3]
```

---

### Object Spread

The spread operator can also be used to copy properties from one object into another or to merge objects.

### Syntax

```javascript
const newObject = { ...object };
```

### Example

```javascript
const person = { name: "Alice", age: 30 };
const address = { city: "New York", zip: "10001" };
const personWithAddress = { ...person, ...address };

console.log(personWithAddress); 
// Output: { name: "Alice", age: 30, city: "New York", zip: "10001" }
```

🔹 **Copying an object**:

```javascript
const originalPerson = { name: "John", age: 25 };
const personCopy = { ...originalPerson };

console.log(personCopy); // Output: { name: "John", age: 25 }
```

---

### Merging Objects

You can use the spread operator to merge objects. The properties of later objects will **override** those of earlier ones if there are key conflicts.

```javascript
const defaultSettings = { theme: "light", language: "en" };
const userSettings = { language: "es" };
const settings = { ...defaultSettings, ...userSettings };

console.log(settings); // Output: { theme: "light", language: "es" }
```

---

## 3️⃣ Rest Operator (`...`)

The **rest operator** is used to collect multiple elements into an array, usually in function parameters or when destructuring arrays or objects.

### Rest in Function Parameters

The rest operator allows you to collect multiple arguments passed to a function into an array.

### Syntax

```javascript
function func(...args) {
  // args will be an array of all arguments passed
}
```

### Example

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // Output: 10
```

🔹 In this example, the rest operator (`...numbers`) collects all the arguments passed to the `sum` function into the `numbers` array.

---

### Rest in Destructuring

The rest operator can also be used in **array** or **object destructuring** to collect the remaining elements into a new array or object.

#### Array Destructuring with Rest

```javascript
const fruits = ["apple", "banana", "cherry", "date"];
const [firstFruit, secondFruit, ...remainingFruits] = fruits;

console.log(remainingFruits); // Output: ["cherry", "date"]
```

#### Object Destructuring with Rest

```javascript
const person = { name: "Alice", age: 30, city: "New York" };
const { name, ...rest } = person;

console.log(rest); // Output: { age: 30, city: "New York" }
```

---

## 4️⃣ Key Differences between Spread and Rest

* **Spread (`...`)**:

  * Unpacks elements of an array or properties of an object.

  * Typically used in **arrays** or **objects** for copying, merging, or expanding data.

* **Rest (`...`)**:

  * Collects multiple values into an array or object.

  * Used in **function parameters** or **destructuring** to gather remaining elements.

---

## Summary

* **Destructuring**: A convenient way to unpack values from arrays and objects into variables.

  * **Array Destructuring**: Extracts values from arrays into variables.

  * **Object Destructuring**: Extracts properties from objects into variables.

* **Spread Operator (`...`)**: Copies or merges arrays/objects into new ones.

  * **Array Spread**: Used to copy or merge arrays.

  * **Object Spread**: Used to copy or merge object properties.

* **Rest Operator (`...`)**: Collects multiple values into a single array or object.

  * **Rest in Function Parameters**: Gathers all function arguments into an array.

  * **Rest in Destructuring**: Collects remaining elements in arrays or objects.

These features make JavaScript code cleaner, more readable, and easier to work with, especially when handling arrays and objects.

