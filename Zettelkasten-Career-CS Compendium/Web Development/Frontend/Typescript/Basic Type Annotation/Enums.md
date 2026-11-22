---
tags:
  - FORMAT
  - Typescript
---
# TypeScript Enums (`enum`)

An **enum** (short for "enumeration") in TypeScript is a way to **define a set of named constants**. Enums help make code more readable by giving meaningful names to numeric or string values.

---

 Defining a Numeric Enum

By default, TypeScript assigns **numeric values** starting from `0`.

# Example: Basic Numeric Enum

```
enum Color {
    Red,    // 0
    Green,  // 1
    Blue    // 2
}

console.log(Color.Red);   // Output: 0
console.log(Color.Green); // Output: 1
console.log(Color.Blue);  // Output: 2
```

*   
  If no values are assigned, the first member (`Red`) starts at `0`, and the rest increment automatically.

---

 Customizing Enum Values

You can set a **custom starting value**.

# Example: Custom Numeric Enum

```
enum Color {
    Red = 10,
    Green,  // 11 (automatically increments)
    Blue    // 12
}

console.log(Color.Green); // Output: 11
console.log(Color.Blue);  // Output: 12
```

*   
  The next members (`Green`, `Blue`) **increment automatically** from `10`.

You can also assign **specific values** to all members.

```
enum StatusCode {
    Success = 200,
    NotFound = 404,
    ServerError = 500
}

console.log(StatusCode.NotFound); // Output: 404
```

---

 String Enums

Instead of numbers, you can assign **string values**.

# Example: String Enum

```
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT"
}

console.log(Direction.Up);   // Output: "UP"
console.log(Direction.Left); // Output: "LEFT"
```

*   
  String enums do **not** auto-increment; each value must be **explicitly assigned**.

---

 Heterogeneous Enums (Mixing String & Number)

Although not common, enums can mix **string and numeric** values.

```
enum MixedEnum {
    Yes = "YES",
    No = 0
}

console.log(MixedEnum.Yes); // Output: "YES"
console.log(MixedEnum.No);  // Output: 0
```

---

 Reverse Mapping (Numeric Enums Only)

TypeScript allows **reverse mapping** for numeric enums, meaning you can look up the name by its value.

```
enum Color {
    Red = 1,
    Green = 2,
    Blue = 3
}

console.log(Color[2]);  // Output: "Green"
console.log(Color.Green); // Output: 2
```

*   
  This does **not** work for string enums.

---

 Using `const enum` (Performance Optimization)

Using `const enum` **removes the enum from the compiled JavaScript output**, making the code more efficient.

```
const enum Size {
    Small = 1,
    Medium = 2,
    Large = 3
}

console.log(Size.Medium); // Output: 2
```

*   
  In the compiled JavaScript, `Size.Medium` is directly replaced with `2`, eliminating extra enum lookup code.

---

 Enum Use Cases

✅ **Better readability** than using raw numbers or strings.  
 ✅ **Prevents invalid values** from being used.  
 ✅ **Useful for defining states, colors, directions, etc.**

# Example: Using Enums in Functions

```
enum Status {
    Success = 200,
    NotFound = 404,
    Error = 500
}

function handleResponse(status: Status) {
    if (status === Status.Success) {
        console.log("Request was successful!");
    } else if (status === Status.NotFound) {
        console.log("Resource not found!");
    }
}

handleResponse(Status.Success); // Output: "Request was successful!"
```

---

 Enum vs. Object

| Feature | Enum (`enum`) | Object (`const obj = {}`) |
| ----- | ----- | ----- |
| Auto-Increment | ✅ Yes (numeric enums) | ❌ No |
| Reverse Lookup | ✅ Yes (numeric enums) | ❌ No |
| Runtime Presence | ✅ Exists at runtime | ✅ Exists at runtime |
| String Values | ✅ Yes | ✅ Yes |
| Performance | ⚠️ Can have extra runtime code | ✅ Optimized |

For better performance in small projects, using **objects** instead of enums might be preferred.

```
const Status = {
    Success: 200,
    NotFound: 404,
    Error: 500
} as const;

console.log(Status.Success); // 200
```

---

 Summary

| Enum Type | Example | Output |
| ----- | ----- | ----- |
| **Numeric Enum** | `enum Color { Red, Green, Blue }` | `Color.Red → 0` |
| **Custom Values** | `enum Status { OK = 200, NotFound = 404 }` | `Status.NotFound → 404` |
| **String Enum** | `enum Direction { Up = "UP" }` | `Direction.Up → "UP"` |
| **Reverse Lookup** | `Color[1]` | `"Red"` (Only for numeric enums) |
| **Const Enum** | `const enum Size { Small = 1 }` | Faster, removes extra code |

---

Would you like a deeper dive into any specific enum feature? 😊

