---
tags:
  - FORMAT
  - Typescript
---
Absolutely\! **Type guards** in TypeScript help you **narrow down the type** of a variable at runtime, so you can write safer, more type-aware code.

---

# What Are Type Guards?

A **type guard** is a runtime check that ensures a variable is of a certain type. Once TypeScript detects that a value has passed a type guard, it narrows the type for the rest of the scope.

There are a few main kinds:

---

## `typeof` Type Guards

Use `typeof` for **primitive types**: `string`, `number`, `boolean`, `bigint`, `symbol`, `undefined`, and `function`.

```
function printValue(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase()); // value is string here
  } else {
    console.log(value.toFixed(2)); // value is number here
  }
}
```

---

## `instanceof` Type Guards

Use `instanceof` to check if an object is an instance of a **class**.

```
class Dog {
  bark() {
    console.log("Woof!");
  }
}

class Cat {
  meow() {
    console.log("Meow!");
  }
}

function speak(pet: Dog | Cat) {
  if (pet instanceof Dog) {
    pet.bark(); // pet is Dog here
  } else {
    pet.meow(); // pet is Cat here
  }
}
```

---

## Custom Type Predicates

You can define your own **type guard functions** using the `parameter is Type` return syntax. These are super useful for complex or interface-based types.

```
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  if (isFish(pet)) {
    pet.swim(); // pet is Fish here
  } else {
    pet.fly(); // pet is Bird here
  }
}
```

*   
  `pet is Fish` is a **type predicate**.

* TypeScript uses it to narrow the type within the `if` block.

---

## Bonus: Discriminated Unions \+ `in` operator

Another common guard: using a shared key across union types.

```
type Square = { kind: "square"; size: number };
type Circle = { kind: "circle"; radius: number };

function area(shape: Square | Circle) {
  if ("size" in shape) {
    return shape.size ** 2;
  } else {
    return Math.PI * shape.radius ** 2;
  }
}
```

Or use the **discriminant** (`kind`) directly:

```
function area(shape: Square | Circle) {
  switch (shape.kind) {
    case "square":
      return shape.size ** 2;
    case "circle":
      return Math.PI * shape.radius ** 2;
  }
}
```

---

## Summary

| Type Guard | Use For | Example Check |
| ----- | ----- | ----- |
| `typeof` | Primitives | `typeof x === "string"` |
| `instanceof` | Class instances | `x instanceof Dog` |
| Custom predicate | Interfaces / complex objects | `isFish(pet): pet is Fish` |
| `in` | Property existence check | `"swim" in pet` |
| Discriminated unions | Tagged object shapes | `switch (shape.kind)` |

---

Let me know if you want a real-world use case or a deeper dive into custom type predicates\!

