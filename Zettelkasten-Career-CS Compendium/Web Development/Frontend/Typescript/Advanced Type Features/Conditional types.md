---
tags:
  - FORMAT
  - Typescript
---
Definitely\! **Conditional types** in TypeScript let you define types that **depend on a condition**, kind of like an `if/else` statement—but for types.

---

# ✅ Syntax

```
T extends U ? X : Y
```

This means:

* If type `T` is assignable to type `U`, then use type `X`

* Otherwise, use type `Y`

---

# ✅ Basic Example

```
type IsString<T> = T extends string ? "yes" : "no";

type A = IsString<string>;  // "yes"
type B = IsString<number>;  // "no"
```

Explanation:

* If `T` is a `string`, the type resolves to `"yes"`, otherwise to `"no"`.

---

# ✅ Real-World Example: Removing `null` and `undefined`

```
type NonNullable<T> = T extends null | undefined ? never : T;

type A = NonNullable<string | null>;   // string
type B = NonNullable<number | undefined>; // number
```

This is actually how TypeScript's built-in `NonNullable` utility type works\!

---

# ✅ With Generics

```
type GetReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function greet(): string {
  return "Hello";
}

type Result = GetReturnType<typeof greet>; // string
```

Here, `infer R` lets you **capture** and **reuse** the return type of the function.

---

# ✅ Conditional Type with Arrays

```
type ElementType<T> = T extends (infer U)[] ? U : T;

type A = ElementType<string[]>; // string
type B = ElementType<number>;   // number
```

If `T` is an array, return its element type, otherwise return `T` as-is.

---

# ✅ Conditional Types \+ Discriminated Unions

```
type Animal = { type: "dog" | "cat" };
type Speak<T> = T extends { type: "dog" } ? "bark" : "meow";

type A = Speak<{ type: "dog" }>; // "bark"
type B = Speak<{ type: "cat" }>; // "meow"
```

---

# ✅ Summary

| Part | Meaning |
| ----- | ----- |
| `T extends U` | Check if `T` is assignable to `U` |
| `? X` | If yes, result is type `X` |
| `: Y` | If no, result is type `Y` |

Conditional types are **powerful** for building flexible, reusable type logic.

---

Want to see how conditional types are used in common utility types or real-world libraries?

