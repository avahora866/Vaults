---
tags:
  - FORMAT
  - Typescript
---
# TypeScript Primitive Types

TypeScript provides several **primitive types** that are built into the language. These are the most basic types used to define variables.

---

# `string` (Textual Data)

* Represents textual data enclosed in **single (`'`), double (`"`), or backticks (`` ` ``)**

* Example:

```
let message: string = "Hello, TypeScript!";
console.log(message);
```

---

# `number` (Numeric Values)

* Represents both **integers** and **floating-point numbers**.

* Supports **decimal (`10`), hexadecimal (`0xA`), binary (`0b1010`), and octal (`0o12`)**.

* Example:

```
let count: number = 42;
let price: number = 99.99;
let hex: number = 0xff;
let binary: number = 0b1010;
let octal: number = 0o12;
```

---

# `boolean` (True/False)

* Represents **true** or **false** values.

* Example:

```
let isDone: boolean = true;
let hasPermission: boolean = false;
```

---

# `null` (Empty Value)

* Represents an **intentional absence of value**.

* Can be assigned explicitly.

* Example:

```
let emptyValue: null = null;
```

---

# `undefined` (Uninitialized Value)

* Represents a **variable that has been declared but not assigned a value**.

* Example:

```
let notAssigned: undefined;
console.log(notAssigned); // Output: undefined
```

---

# `bigint` (Large Integers)

* Used to store **very large integer values** beyond the range of `number`.

* Created by adding `n` at the end of a number.

* Example:

```
let bigNumber: bigint = 9007199254740991n;
let anotherBigInt: bigint = BigInt(12345678901234567890);
```

---

# `symbol` (Unique Identifiers)

* Used to create **unique values** that cannot be accidentally overwritten.

* Example:

```
let uniqueKey: symbol = Symbol("id");
let anotherKey: symbol = Symbol("id");

console.log(uniqueKey === anotherKey); // Output: false (Symbols are always unique)
```

---

# Summary Table

| Type | Example | Description |
| ----- | ----- | ----- |
| `string` | `"Hello"` or `'World'` | Textual data |
| `number` | `42`, `3.14`, `0xff` | Numbers (int & float) |
| `boolean` | `true`, `false` | True/False values |
| `null` | `null` | Empty value |
| `undefined` | `undefined` | Variable not assigned |
| `bigint` | `1234567890123456789n` | Large integers |
| `symbol` | `Symbol("id")` | Unique identifiers |

Would you like a deeper explanation of any specific type? 😊

