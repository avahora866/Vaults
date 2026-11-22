---
tags:
  - FORMAT
  - Typescript
---
# TypeScript Special Types: `any`, `unknown`, `never`, `void`

TypeScript provides special types that handle different kinds of values in unique ways:

* `any` → Accepts **any** type (unsafe, avoids type checking).

* `unknown` → Accepts **any** type but requires **type checking** before usage.

* `void` → Represents **no return value** (used in functions).

* `never` → Represents values that **never occur** (used for errors or infinite loops).

---

 `any` (Disable Type Checking \- Unsafe)

The `any` type allows a variable to hold **any** value without type checking.  
 🚨 **Avoid using `any` unless absolutely necessary** (it bypasses TypeScript’s safety features).

# Example: Using `any`

```
let value: any;

value = "Hello";   // ✅ Allowed (string)
value = 42;        // ✅ Allowed (number)
value = true;      // ✅ Allowed (boolean)

console.log(value.toUpperCase()); // No TypeScript error, but might fail at runtime!
```

*   
  `any` **disables** TypeScript's strict type checking.

* Use it **only** when dealing with **third-party libraries** or **dynamic values**.

---

 `unknown` (Safer Alternative to `any`)

The `unknown` type also accepts **any** value but **prevents direct usage** without type checking.  
 ✅ **Safer than `any`** because it **requires type checking** before use.

# Example: Using `unknown`

```
let input: unknown;

input = "Hello";
input = 42;

// input.toUpperCase(); // ❌ Error: Object is of type 'unknown'

// Correct way: Type checking before usage
if (typeof input === "string") {
    console.log(input.toUpperCase()); // ✅ Allowed after type check
}
```

*   
  `unknown` **forces type checking**, unlike `any`.

* Useful when **receiving unknown values** (e.g., user input, API responses).

---

 `void` (No Return Value)

The `void` type is used for **functions that do not return a value**.

# Example: Using `void` in Functions

```
function logMessage(message: string): void {
    console.log(message);
}

logMessage("Hello, TypeScript!"); // Output: Hello, TypeScript!
```

*   
  Functions with `void` **must not return a value**.

* Trying to return a value in a `void` function causes an error.

```
function invalidFunction(): void {
    return "Hello"; // ❌ Error: Type 'string' is not assignable to 'void'.
}
```

---

 `never` (Impossible to Happen)

The `never` type is used for functions that **never return a value**.  
 It’s often used for **errors, infinite loops, or unreachable code**.

# Example: Using `never` in a Function

```
function throwError(message: string): never {
    throw new Error(message);
}

throwError("Something went wrong!"); // Function execution stops here
```

*   
  `never` is used for functions that **always throw an error** or **never terminate**.

# Example: Infinite Loop with `never`

```
function infiniteLoop(): never {
    while (true) {
        console.log("This never ends...");
    }
}
```

*   
  This function **never exits**, so it has a return type of `never`.

---

 Comparison Table

| Type | Description | Example |
| ----- | ----- | ----- |
| `any` | Accepts any type, no type checking (unsafe) | `let x: any = "Hello"; x = 42;` |
| `unknown` | Accepts any type, but requires type checking before usage (safe) | `if (typeof x === "string") { x.toUpperCase(); }` |
| `void` | Used for functions that return nothing | `function log(): void { console.log("Hi"); }` |
| `never` | Used for functions that never return (errors, infinite loops) | `function fail(): never { throw new Error("Fail"); }` |

---

# When to Use Each Type

✅ Use `any` → When you **don’t care** about type safety (avoid if possible).  
 ✅ Use `unknown` → When handling **dynamic values** safely.  
 ✅ Use `void` → For functions that **don’t return anything**.  
 ✅ Use `never` → For functions that **never return** (errors, infinite loops).

Would you like deeper explanations with real-world examples? 😊

