---
tags:
  - FORMAT
  - JavaScript
---
# Spread and Rest Operators in JavaScript

The **spread** and **rest** operators are represented by the same syntax (`...`), but they are used in different contexts with distinct purposes. These operators allow you to handle arrays, objects, and function parameters more efficiently, reducing the need for boilerplate code.

---

## 1️⃣ Spread Operator (`...`)

The **spread operator** allows you to expand or "spread" elements of an iterable (such as an array or object) into individual elements. This is useful for copying arrays, combining arrays, or spreading object properties into another object.

### Syntax

```javascript
[...iterable]
```

### Spread with Arrays

The spread operator can be used to unpack elements from an array or combine arrays.

#### Example 1: Copying an Array

```javascript
let arr1 = [1, 2, 3];
let arr2 = [...arr1];  // Creates a shallow copy of arr1

console.log(arr2);  // Output: [1, 2, 3]
```

#### Example 2: Merging Arrays

You can merge two or more arrays using the spread operator.

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let mergedArr = [...arr1, ...arr2];

console.log(mergedArr);  // Output: [1, 2, 3, 4, 5, 6]
```

### Spread with Objects

The spread operator can also be used to copy or merge object properties.

#### Example 1: Copying an Object

```javascript
let obj1 = { name: "Alice", age: 25 };
let obj2 = { ...obj1 };  // Creates a shallow copy of obj1

console.log(obj2);  // Output: { name: "Alice", age: 25 }
```

#### Example 2: Merging Objects

You can merge two or more objects into one.

```javascript
let obj1 = { name: "Alice" };
let obj2 = { age: 25 };
let mergedObj = { ...obj1, ...obj2 };

console.log(mergedObj);  // Output: { name: "Alice", age: 25 }
```

### Spread with Function Arguments

You can use the spread operator to pass elements of an array as individual arguments to a function.

```javascript
function sum(a, b, c) {
  return a + b + c;
}

let numbers = [1, 2, 3];
console.log(sum(...numbers));  // Output: 6
```

In this example, the array `numbers` is "spread" into individual arguments for the `sum` function.

---

## 2️⃣ Rest Operator (`...`)

The **rest operator** is used in function parameters or when destructuring arrays or objects to collect multiple values into a single variable. It allows you to handle an unknown number of arguments or properties.

### Syntax

```javascript
function func(...args) {
  // args is an array containing all arguments passed to the function
}
```

### Rest in Function Parameters

The rest operator can be used to collect a variable number of arguments passed to a function into an array.

#### Example: Rest in Function Parameters

```javascript
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4));  // Output: 10
console.log(sum(5, 6));        // Output: 11
```

In this example, the rest operator collects all the arguments into the `numbers` array, allowing the function to handle any number of arguments.

### Rest in Array Destructuring

The rest operator can be used in array destructuring to collect the remaining elements into a new array.

#### Example: Rest in Array Destructuring

```javascript
let arr = [1, 2, 3, 4, 5];
let [first, second, ...rest] = arr;

console.log(first);   // Output: 1
console.log(second);  // Output: 2
console.log(rest);    // Output: [3, 4, 5]
```

In this case, `first` and `second` receive the first two values, and the remaining elements are collected into the `rest` array.

### Rest in Object Destructuring

You can also use the rest operator with object destructuring to collect remaining properties into a new object.

#### Example: Rest in Object Destructuring

```javascript
let person = { name: "Alice", age: 25, city: "Wonderland" };
let { name, ...otherInfo } = person;

console.log(name);       // Output: Alice
console.log(otherInfo);  // Output: { age: 25, city: "Wonderland" }
```

Here, the `name` property is extracted, and the rest of the properties are collected into the `otherInfo` object.

---

## 3️⃣ Key Differences Between Spread and Rest

Although both operators use the same syntax (`...`), they serve different purposes depending on the context.

| Context | Purpose | Example |
| ----- | ----- | ----- |
| **Spread** (arrays, objects) | Unpacks elements or properties | `let arr2 = [...arr1]` |
| **Rest** (function parameters, destructuring) | Collects remaining values into a single variable | `function func(...args)` |

### Summary of Spread

* **In arrays**: Expands elements (e.g., for copying or merging arrays).

* **In objects**: Expands properties (e.g., for copying or merging objects).

* **In function calls**: Spreads array elements as arguments to a function.

### Summary of Rest

* **In function parameters**: Collects multiple arguments into an array.

* **In array destructuring**: Collects the remaining elements into an array.

* **In object destructuring**: Collects remaining properties into an object.

---

## 4️⃣ Combined Usage of Spread and Rest

You can combine the spread and rest operators for more complex operations.

### Example: Using Spread and Rest Together in Objects

```javascript
let person = { name: "Alice", age: 25, city: "Wonderland" };
let updatedPerson = { ...person, age: 26, country: "Dreamland" };

console.log(updatedPerson); // Output: { name: "Alice", age: 26, city: "Wonderland", country: "Dreamland" }
```

In this case:

* We use **spread** to copy all properties from `person` into `updatedPerson`.

* We then overwrite the `age` property and add a new property `country`.

### Example: Rest and Spread Together in Arrays

```javascript
let numbers = [1, 2, 3, 4, 5];
let [first, second, ...rest] = numbers;

let combined = [...rest, first, second];
console.log(combined);  // Output: [3, 4, 5, 1, 2]
```

Here:

* We use **rest** to capture the remaining elements of `numbers` into `rest`.

* We then use **spread** to create a new array `combined`, rearranging the elements.

---

## 5️⃣ Conclusion

* **Spread operator (`...`)**: Expands elements (arrays) or properties (objects) into individual values. It is useful for copying, merging, or passing elements as arguments.

* **Rest operator (`...`)**: Collects multiple values (function arguments, array elements, or object properties) into a single variable or array/object.

These operators make working with arrays, objects, and functions in JavaScript much simpler, more flexible, and concise, improving code readability and reducing boilerplate.

