---
tags:
  - FORMAT
  - Typescript
---
# TypeScript Tuples (`[string, number]`)

A **tuple** in TypeScript is a **fixed-length** and **ordered** array where each element has a specific type. Tuples allow you to store multiple values of different types in a single variable.

---

 Defining a Tuple

A tuple is declared using **square brackets** (`[]`), with each element’s type explicitly defined.

# Example: Basic Tuple

```
let person: [string, number] = ["Alice", 25];

console.log(person[0]); // Output: "Alice" (string)
console.log(person[1]); // Output: 25 (number)
```

*   
  `person[0]` is a `string`

* `person[1]` is a `number`

---

 Tuple with More Than Two Values

Tuples can have more than two elements with different types.

# Example: Extended Tuple

```
let employee: [number, string, boolean] = [101, "John Doe", true];

console.log(employee[0]); // 101 (number)
console.log(employee[1]); // "John Doe" (string)
console.log(employee[2]); // true (boolean)
```

---

 Accessing and Modifying Tuple Elements

You can access and modify tuple elements using **indexing**.

# Example: Updating Tuple Values

```
let car: [string, number] = ["Tesla", 2024];

// Updating values
car[0] = "BMW";
car[1] = 2022;

console.log(car); // Output: ["BMW", 2022]
```

---

 Tuple with Optional Elements

You can define tuples where some elements are optional using `?`.

# Example: Optional Tuple Elements

```
let user: [string, number?] = ["Alice"];

console.log(user[0]); // Output: "Alice"
console.log(user[1]); // Output: undefined
```

*   
  The `number` element is optional.

---

 Tuple with `readonly` (Immutable)

To prevent modification, you can use `readonly`.

# Example: Readonly Tuple

```
let coordinates: readonly [number, number] = [40.7128, -74.0060];

// coordinates[0] = 50; // Error: Cannot assign to '0' because it is a read-only property.
```

---

 Using Tuples with `push()` and `pop()`

TypeScript allows pushing and popping elements, which can cause unintended behavior.

# Example: Tuple with `push()`

```
let student: [string, number] = ["John", 18];

student.push(90); // Allowed! But should be avoided.
console.log(student); // Output: ["John", 18, 90]
```

*   
  Although `student` is declared as `[string, number]`, TypeScript **does not strictly enforce length** when using `push()`.

* To prevent this, use **readonly tuples**.

---

 Named Tuples (Improving Readability)

You can give **names** to tuple elements for better readability.

# Example: Named Tuple

```
let product: [id: number, name: string, price: number] = [1, "Laptop", 999];

console.log(product[0]); // 1
console.log(product[1]); // "Laptop"
console.log(product[2]); // 999
```

This improves code clarity when working with tuples.

---

 Tuple in Function Return Types

Tuples are often used as **return types** in functions.

# Example: Returning Multiple Values Using a Tuple

```
function getUser(): [string, number] {
    return ["Alice", 30];
}

let user = getUser();
console.log(user[0]); // "Alice"
console.log(user[1]); // 30
```

*   
  Instead of returning an object, a tuple provides a **compact** way to return multiple values.

---

 Tuple vs. Array

| Feature | Tuple (`[string, number]`) | Array (`string[]`) |
| ----- | ----- | ----- |
| Length Fixed | ✅ Yes | ❌ No |
| Mixed Types | ✅ Yes | ❌ No (All elements must be the same type) |
| Readability | ✅ Clear structure | ❌ Less clear when multiple data types are needed |

---

 Summary

| Feature | Example |
| ----- | ----- |
| **Basic Tuple** | `let person: [string, number] = ["Alice", 25];` |
| **Multi-Type Tuple** | `let data: [number, string, boolean] = [1, "Data", true];` |
| **Optional Elements** | `let user: [string, number?] = ["Bob"];` |
| **Readonly Tuple** | `let coord: readonly [number, number] = [10, 20];` |
| **Named Tuple** | `let product: [id: number, name: string] = [1, "Laptop"];` |
| **Function Return Tuple** | `function getUser(): [string, number] { return ["Tom", 40]; }` |

Would you like more examples or a deeper dive into any of these topics? 😊

