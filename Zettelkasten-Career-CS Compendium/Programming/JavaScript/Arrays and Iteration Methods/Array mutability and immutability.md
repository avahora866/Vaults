---
tags:
  - FORMAT
  - JavaScript
---
# Array Mutability and Immutability in JavaScript

In JavaScript, arrays are **mutable** by default, meaning their contents can be changed after they are created. However, understanding when an array operation modifies the array itself (mutability) and when it returns a new array without modifying the original (immutability) is crucial for writing predictable and bug-free code.

Here’s an explanation of some common array methods and whether they are **mutable** (change the original array) or **immutable** (return a new array).

---

## 1️⃣ Mutable Methods

### `push()`

The `push()` method **mutates** the array by adding one or more elements to the **end** of the array and returns the new length of the array.

### Example:

```javascript
const fruits = ["apple", "banana"];
fruits.push("cherry"); // Mutates the original array

console.log(fruits); // Output: ["apple", "banana", "cherry"]
```

🔹 **Mutability**: `push()` modifies the original array by adding elements to it.

---

### `pop()`

The `pop()` method **mutates** the array by removing the **last element** of the array and returns the removed element.

### Example:

```javascript
const fruits = ["apple", "banana", "cherry"];
const lastFruit = fruits.pop(); // Mutates the original array

console.log(fruits); // Output: ["apple", "banana"]
console.log(lastFruit); // Output: "cherry"
```

🔹 **Mutability**: `pop()` changes the original array by removing the last element.

---

### `shift()`

The `shift()` method **mutates** the array by removing the **first element** of the array and returns the removed element.

### Example:

```javascript
const fruits = ["apple", "banana", "cherry"];
const firstFruit = fruits.shift(); // Mutates the original array

console.log(fruits); // Output: ["banana", "cherry"]
console.log(firstFruit); // Output: "apple"
```

🔹 **Mutability**: `shift()` modifies the original array by removing the first element.

---

### `unshift()`

The `unshift()` method **mutates** the array by adding one or more elements to the **beginning** of the array and returns the new length of the array.

### Example:

```javascript
const fruits = ["banana", "cherry"];
fruits.unshift("apple"); // Mutates the original array

console.log(fruits); // Output: ["apple", "banana", "cherry"]
```

🔹 **Mutability**: `unshift()` changes the original array by adding elements to the beginning.

---

### `splice()`

The `splice()` method **mutates** the array by adding or removing elements at any position in the array.

* Syntax: `array.splice(start, deleteCount, item1, item2, ...)`

  * `start`: The index at which to start changing the array.

  * `deleteCount`: The number of elements to remove.

  * `item1, item2, ...`: The elements to add.

### Example:

```javascript
const fruits = ["apple", "banana", "cherry"];
fruits.splice(1, 1, "orange", "grape"); // Removes 1 element at index 1 and adds "orange" and "grape"

console.log(fruits); // Output: ["apple", "orange", "grape", "cherry"]
```

🔹 **Mutability**: `splice()` modifies the original array by removing, adding, or replacing elements.

---

## 2️⃣ Immutable Methods

### `slice()`

The `slice()` method **does not mutate** the original array. It **returns a shallow copy** of the portion of the array from the specified start index to the specified end index.

* Syntax: `array.slice(start, end)`

  * `start`: The beginning index (inclusive).

  * `end`: The end index (exclusive).

### Example:

```javascript
const fruits = ["apple", "banana", "cherry"];
const newFruits = fruits.slice(1, 3); // Returns a new array from index 1 to 2

console.log(fruits); // Output: ["apple", "banana", "cherry"] (original array is unchanged)
console.log(newFruits); // Output: ["banana", "cherry"]
```

🔹 **Immutability**: `slice()` creates a new array and does not affect the original array.

---

## 3️⃣ Comparison of Mutability and Immutability

| Method | Mutability | Description |
| ----- | ----- | ----- |
| `push()` | Mutable | Adds one or more elements to the end of the array. |
| `pop()` | Mutable | Removes the last element from the array. |
| `shift()` | Mutable | Removes the first element from the array. |
| `unshift()` | Mutable | Adds one or more elements to the beginning of the array. |
| `splice()` | Mutable | Adds or removes elements at any position in the array. |
| `slice()` | Immutable | Returns a shallow copy of a portion of the array without modifying the original array. |

---

## 4️⃣ Best Practices and Use Cases

### When to Use Mutable Methods

Mutable methods are often used when you want to directly modify the original array. For example:

* **Modifying** an array in place.

* **Adding or removing** elements from the array without needing to retain the original array.

### When to Use Immutable Methods

Immutable methods are useful in functional programming, where you want to avoid changing the original array. This can help with:

* **Preserving immutability**: Ensuring that the original array remains unchanged for predictable code behavior.

* **Avoiding side effects**: Preventing accidental mutations of data, especially in complex applications or when working with frameworks like React (which relies on immutability for performance optimizations).

* **Returning new arrays**: When you need to return new arrays without modifying the original one.

---

## 5️⃣ Summary of Array Mutability vs Immutability

* **Mutable Methods**: Modify the original array and return either the modified array or a specific element (e.g., `push()`, `pop()`, `shift()`, `unshift()`, `splice()`).

* **Immutable Methods**: Do not modify the original array but return a new array (e.g., `slice()`).

Understanding the difference between mutable and immutable methods helps you choose the right approach depending on whether or not you need to preserve the original array. For predictable and safe code, **immutability** is often preferred, but **mutability** is useful when you need to modify the array in place.

