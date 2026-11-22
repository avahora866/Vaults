---
tags:
  - FORMAT
  - Typescript
---
Absolutely\! **Template literal types** in TypeScript are a way to **construct new string literal types** using pattern-based strings—just like JavaScript's template literals, but for types.

---

# ✅ What Are Template Literal Types?

They let you define string types by **combining literal types** with other string/number types using the `${}` syntax.

## Basic Syntax:

```
type Greeting = `Hello ${string}`;
```

This means: `Greeting` can be *any string* that starts with `"Hello "` followed by any other string.

---

# ✅ Basic Example

```
type Color = "red" | "blue";
type Paint = `dark-${Color}`;

// Result:
// Paint = "dark-red" | "dark-blue"
```

You're combining the prefix `"dark-"` with each member of the union type `"red"` and `"blue"`.

---

# ✅ Combining Multiple Parts

```
type Size = "S" | "M" | "L";
type Color = "red" | "blue";

type Variant = `${Size}-${Color}`;
// Result: "S-red" | "S-blue" | "M-red" | "M-blue" | "L-red" | "L-blue"
```

You’re generating all combinations of size and color as literal strings.

---

# ✅ With String Interpolation

You can use generic `string` or `number` types too:

```
type UserID = `user-${number}`;

// Example valid values:
const a: UserID = "user-123";  // ✅
const b: UserID = "admin-123"; // ❌ Error
```

This enforces that the value must begin with `"user-"` followed by a number.

---

# ✅ Real-World Use Case: Route Paths

```
type Route = `/users/${string}`;

// Valid:
const a: Route = "/users/john";  // ✅
// Invalid:
const b: Route = "/posts/42";    // ❌
```

This is useful in API typings or router systems to define valid path patterns.

---

# ✅ Combining With Conditional Types

You can make even more powerful types when combining template literals with conditionals:

```
type EventName<T extends string> = `on${Capitalize<T>}`;

type ClickEvent = EventName<"click">; // "onClick"
type HoverEvent = EventName<"hover">; // "onHover"
```

Here, `Capitalize<T>` is a **string manipulation utility type** that capitalizes the first letter.

---

# ✅ Summary

Template literal types are used to:

* Construct complex string types

* Build dynamic keys or patterns

* Enforce specific string formats

* Combine with union types to generate all valid string combinations

## Pattern:

```
type NewType = `${Prefix}${SomeType}${Suffix}`
```

They make your types more expressive, especially in places like route patterns, event names, style systems, and key generators.

Let me know if you'd like to explore how to combine these with mapped types or key remapping\!

