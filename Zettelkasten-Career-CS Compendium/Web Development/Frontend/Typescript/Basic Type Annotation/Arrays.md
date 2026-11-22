---
tags:
  - FORMAT
  - Typescript
---
# TypeScript Arrays

In TypeScript, arrays can be defined using two different syntaxes:

1. **Square Bracket Notation (`type[]`)**

2. **Generic Array Notation (`Array<type>`)**

---

 Defining Arrays in TypeScript

# Using `type[]` notation

```
let numbers: number[] = [1, 2, 3, 4, 5];
let names: string[] = ["Alice", "Bob", "Charlie"];
```

# Using `Array<type>` notation

```
let numbers: Array<number> = [10, 20, 30];
let names: Array<string> = ["John", "Jane"];
```

Both syntaxes work the same way, so choosing one is a matter of preference.

---

 Common Array Operations in TypeScript

# Adding and Removing Elements

```
let fruits: string[] = ["Apple", "Banana"];

// Add an element
fruits.push("Cherry"); // ["Apple", "Banana", "Cherry"]

// Remove the last element
fruits.pop(); // ["Apple", "Banana"]
```

# Accessing Elements

```
let numbers: number[] = [10, 20, 30];
console.log(numbers[1]); // Output: 20
```

# Iterating Over an Array

Using `for` loop:

```
let colors: string[] = ["Red", "Green", "Blue"];
for (let i = 0; i < colors.length; i++) {
    console.log(colors[i]);
}
```

Using `forEach`:

```
colors.forEach(color => console.log(color));
```

Using `map`:

```
let uppercaseColors = colors.map(color => color.toUpperCase());
console.log(uppercaseColors); // ["RED", "GREEN", "BLUE"]
```

---

 Multi-Dimensional Arrays

You can create **arrays of arrays** (multi-dimensional arrays).

# Example: 2D Number Array

```
let matrix: number[][] = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

console.log(matrix[1][2]); // Output: 6
```

---

 Readonly Arrays

If you want to **prevent modifications** to an array, you can use `readonly`.

```
let readonlyNumbers: readonly number[] = [1, 2, 3];
// readonlyNumbers.push(4); // Error: Property 'push' does not exist on type 'readonly number[]'.
```

---

 Tuple (Fixed-Length Array with Different Types)

If you need an array with **fixed size and different types**, use **tuples**.

```
let person: [string, number] = ["Alice", 25];

console.log(person[0]); // Output: "Alice"
console.log(person[1]); // Output: 25
```

*   
  `person[0]` must always be a `string`.

* `person[1]` must always be a `number`.

---

 Difference Between `number[]` and `Array<number>`

| Syntax | Example | Use Case |
| ----- | ----- | ----- |
| `type[]` | `let nums: number[] = [1, 2, 3];` | Commonly used, simple syntax |
| `Array<type>` | `let nums: Array<number> = [1, 2, 3];` | Used in **generics** and functional programming |

Both approaches are interchangeable, but `Array<type>` is preferred when working with **generic functions**.

---

# Summary

| Feature | Example |
| ----- | ----- |
| **Basic Array** | `let arr: number[] = [1, 2, 3];` |
| **Generic Array** | `let arr: Array<number> = [1, 2, 3];` |
| **Multi-Dimensional** | `let matrix: number[][] = [[1,2], [3,4]];` |
| **Readonly Array** | `let arr: readonly string[] = ["A"];` |
| **Tuple** | `let person: [string, number] = ["Tom", 30];` |

Would you like an example with a specific use case? 😊

