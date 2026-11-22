---
tags:
  - FORMAT
  - JavaScript
---
# Destructuring Assignment in JavaScript

**Destructuring assignment** is a convenient way to extract values from arrays or objects and assign them to variables. This syntax makes it easier to work with complex data structures and simplifies your code, making it cleaner and more readable.

---

## 1️⃣ Array Destructuring

Array destructuring allows you to unpack values from an array into distinct variables. The variables are assigned based on their position in the array.

### Syntax

```javascript
let [variable1, variable2, variable3] = array;
```

### Example

```javascript
let numbers = [10, 20, 30];
let [a, b, c] = numbers;

console.log(a); // Output: 10
console.log(b); // Output: 20
console.log(c); // Output: 30
```

In this example:

* The first element of `numbers` is assigned to `a`, the second to `b`, and the third to `c`.

### Skipping Elements

You can skip elements in the array by leaving the corresponding variable blank.

```javascript
let numbers = [10, 20, 30];
let [a, , c] = numbers;

console.log(a); // Output: 10
console.log(c); // Output: 30
```

Here, the second element (20) is skipped, and `a` and `c` get the first and third values, respectively.

### Default Values

If an array has fewer elements than the number of variables being destructured, you can set default values.

```javascript
let numbers = [10];
let [a, b = 20] = numbers;

console.log(a); // Output: 10
console.log(b); // Output: 20 (default value used)
```

---

## 2️⃣ Object Destructuring

Object destructuring allows you to unpack properties from an object and assign them to variables. The variable names must match the property names.

### Syntax

```javascript
let { key1, key2 } = object;
```

### Example

```javascript
let person = { name: "Alice", age: 25 };
let { name, age } = person;

console.log(name); // Output: Alice
console.log(age);  // Output: 25
```

Here, the `name` and `age` properties from the `person` object are unpacked into variables with the same names.

### Renaming Variables

If you want to assign the object's properties to variables with different names, you can use **colon** syntax to rename them.

```javascript
let person = { name: "Alice", age: 25 };
let { name: fullName, age: yearsOld } = person;

console.log(fullName);  // Output: Alice
console.log(yearsOld);  // Output: 25
```

In this example, `name` is assigned to `fullName` and `age` to `yearsOld`.

### Default Values in Object Destructuring

You can also provide default values for properties that might be `undefined`.

```javascript
let person = { name: "Alice" };
let { name, age = 30 } = person;

console.log(name); // Output: Alice
console.log(age);  // Output: 30 (default value used)
```

---

## 3️⃣ Nested Destructuring

You can destructure nested objects or arrays by following the same pattern, but adding additional destructuring for each level.

### Example with Nested Objects

```javascript
let person = { name: "Alice", address: { city: "Wonderland", zip: "12345" } };
let { name, address: { city, zip } } = person;

console.log(name);  // Output: Alice
console.log(city);  // Output: Wonderland
console.log(zip);   // Output: 12345
```

In this example, `address` is an object, and we destructure its `city` and `zip` properties directly.

### Example with Nested Arrays

```javascript
let numbers = [10, [20, 30]];
let [a, [b, c]] = numbers;

console.log(a); // Output: 10
console.log(b); // Output: 20
console.log(c); // Output: 30
```

Here, `numbers` contains a nested array, and we destructure both the outer and inner arrays.

---

## 4️⃣ Destructuring in Function Parameters

You can also use destructuring in function parameters to directly extract values from objects or arrays passed to the function.

### Example with Object Destructuring in Function

```javascript
function greet({ name, age }) {
  console.log(`Hello, ${name}. You are ${age} years old.`);
}

let person = { name: "Bob", age: 30 };
greet(person); // Output: Hello, Bob. You are 30 years old.
```

### Example with Array Destructuring in Function

```javascript
function sum([a, b]) {
  return a + b;
}

let numbers = [5, 10];
console.log(sum(numbers)); // Output: 15
```

In both examples, we destructure the object and array directly in the function parameters.

---

## 5️⃣ Rest in Destructuring

You can also use the **rest pattern** (`...`) to collect the remaining values into an array (for arrays) or an object (for objects).

### Rest in Arrays

```javascript
let numbers = [1, 2, 3, 4, 5];
let [first, second, ...rest] = numbers;

console.log(first);  // Output: 1
console.log(second); // Output: 2
console.log(rest);   // Output: [3, 4, 5]
```

In this case, the first two elements are assigned to `first` and `second`, and the rest are collected into the `rest` array.

### Rest in Objects

```javascript
let person = { name: "Alice", age: 25, city: "Wonderland" };
let { name, ...others } = person;

console.log(name);    // Output: Alice
console.log(others);  // Output: { age: 25, city: "Wonderland" }
```

Here, `name` is extracted, and the remaining properties are collected into the `others` object.

---

## 6️⃣ Summary

* **Array Destructuring**: Unpacks values from an array into variables based on their position.

* **Object Destructuring**: Unpacks properties from an object into variables with matching names, with support for renaming and default values.

* **Nested Destructuring**: Allows destructuring of deeply nested objects and arrays.

* **Function Parameters**: You can use destructuring directly in function parameters to extract values.

* **Rest Pattern**: Collects remaining values into an array or object.

Destructuring is a powerful and concise tool for working with arrays and objects, especially in complex data structures or when passing data between functions. It improves code readability and reduces boilerplate.

