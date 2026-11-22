---
tags:
  - FORMAT
  - Typescript
---
# Type Assertions in TypeScript (`as` keyword & `<type>value`)

Type assertions allow you to **tell TypeScript** what a value's type is when TypeScript cannot automatically infer it correctly.

🚨 **Type assertions do not change the runtime value; they only tell TypeScript to trust your specified type.**

---

# Using the `as` Keyword (Recommended)

The `as` keyword is the preferred way to **assert a type**.

## Example: Type Assertion with `as`

```
let value: any = "Hello, TypeScript";
let length: number = (value as string).length;

console.log(length); // Output: 16
```

*   
  `value` is of type `any`.

* TypeScript does **not know** it’s a string, so we assert it as `(value as string)`.

* Now, TypeScript treats `value` as a `string` and allows `.length`.

---

# Using the `<type>` Syntax (Alternative)

An older syntax uses angle brackets (`<type>`), but it's **not recommended in JSX/TSX** files because it conflicts with React’s JSX.

## Example: Type Assertion with `<type>`

```
let value: any = "Hello, TypeScript";
let length: number = (<string>value).length;

console.log(length); // Output: 16
```

*   
  Works **identically** to `as string`, but avoid using it in JSX files.

---

# When to Use Type Assertions

## ✅ When TypeScript Cannot Infer the Type Correctly

Example: When selecting an element from the DOM.

```
let input = document.getElementById("username") as HTMLInputElement;
input.value = "John Doe";
```

*   
  `getElementById` returns `HTMLElement | null`, so we **assert it as** `HTMLInputElement` to access `.value`.

## ✅ When Working with `unknown` Type

Example: Ensuring `unknown` values have the correct type before using them.

```
function getData(): unknown {
    return "Hello, world!";
}

let message = getData() as string;
console.log(message.toUpperCase()); // Output: "HELLO, WORLD!"
```

*   
  Since `getData()` returns `unknown`, TypeScript **requires** an assertion before treating it as a string.

---

# Type Assertions vs. Type Casting

🚨 **Type assertions do NOT convert data types at runtime\!**  
 They only affect TypeScript's type checking.

```
let value: any = "123";
let numberValue = value as number; // ❌ This does NOT convert "123" to a number!
console.log(numberValue); // Output: "123" (still a string, NOT a number!)
```

To **actually convert** a type, use `Number()`, `String()`, etc.:

```
let numberValue = Number(value); // ✅ Proper conversion
console.log(numberValue); // Output: 123 (number)
```

---

# Type Assertions with Objects

## Example: Asserting an Object Type

```
type Person = { name: string; age: number };

let user = {} as Person;
user.name = "Alice";
user.age = 30;

console.log(user); // Output: { name: "Alice", age: 30 }
```

*   
  `as Person` tells TypeScript that `user` should be treated as a `Person`.

🚨 **However, be careful\! Type assertions DO NOT validate properties\!**

```
let user = {} as Person;
console.log(user.age); // ❌ No error in TypeScript, but `undefined` at runtime!
```

*   
  The object **doesn’t actually have** `age`, but TypeScript assumes it does.

* This can lead to runtime errors\!

---

# Double Assertions (`as unknown as Type`)

🚨 **Use with caution\!**  
 Sometimes, you may need to **force an invalid type assertion**.

```
let value: any = "Hello";
let num = (value as unknown as number); // 🚨 TypeScript allows it, but it's unsafe!
console.log(num); // Output: "Hello" (not actually a number!)
```

*   
  `value as unknown` → First asserts `unknown` (bypasses strict type checking).

* `as number` → Then asserts `number`.

* ⚠️ **Dangerous because it bypasses TypeScript’s checks completely\!**

---

# Type Assertions vs. Type Guards

🚨 **Type assertions do NOT check types at runtime**\! Use **Type Guards** (`typeof`, `instanceof`) for safer type handling.

## Example: Type Guard (Safer Approach)

```
function processValue(value: unknown) {
    if (typeof value === "string") {
        console.log(value.toUpperCase()); // ✅ Safe
    }
}

processValue("hello"); // Output: "HELLO"
processValue(42); // No error, just ignored
```

*   
  **Safer than type assertion**, because it **actually checks the type** before using it.

---

# Summary Table

| Feature | `as` Keyword | `<type>` Syntax |
| ----- | ----- | ----- |
| Syntax | `value as Type` | `<Type>value` |
| Works in JSX? | ✅ Yes | ❌ No (conflicts with JSX) |
| Type Safety | 🚨 Unsafe (does not check types at runtime) | 🚨 Unsafe (same as `as`) |
| Common Use Cases | DOM elements, `unknown`, API responses | Alternative to `as` (but avoid in JSX) |

---

# Final Thoughts

✅ **Use `as` assertions** for better compatibility.  
 ✅ **Only assert types when necessary** (TypeScript usually infers types correctly).  
 ✅ **Be cautious**—type assertions **don’t actually change** types at runtime\!  
 ✅ **Use type guards (`typeof`, `instanceof`)** if you need runtime type safety.

Would you like more examples with real-world use cases? 😊

