---
tags:
  - FORMAT
  - Typescript
---
Certainly\! Let's dive into **mapped types** in TypeScript.

---

# What Are Mapped Types?

A **mapped type** allows you to transform or create new types by iterating over the properties of an existing type. It's a powerful way to create new types based on existing ones by applying a transformation to each property in the type.

The basic syntax is:

```
{ [K in keyof T]: U }
```

# Breaking Down the Syntax

* `keyof T`: This is a **keyof operator** that gets the keys of type `T`. It's like creating a union of all property names in `T`.

* `K in keyof T`: This is an iteration over all keys of `T`. It means "for each key `K` in the set of keys of `T`."

* `U`: This represents the **type** you want to assign to each key in the resulting type.

Essentially, a mapped type creates a new type by looping over all keys in an existing type and setting their values to a new type (`U`).

---

# Basic Example

```
type Person = {
  name: string;
  age: number;
};

type ReadOnlyPerson = { [K in keyof Person]: Readonly<Person[K]> };

// Result:
// type ReadOnlyPerson = {
//   name: Readonly<string>;
//   age: Readonly<number>;
// }
```

In this example:

* We take `Person` and create a new type `ReadOnlyPerson` where every property is **read-only**.

* The `[K in keyof Person]` part loops through the `name` and `age` keys in the `Person` type and applies the `Readonly` modifier to each.

---

# Using Mapped Types to Modify Property Types

Mapped types allow you to **transform** the type of each property. For instance, you can change the type of every property in a type.

```
type Person = {
  name: string;
  age: number;
};

type StringifiedPerson = { [K in keyof Person]: string };

// Result:
// type StringifiedPerson = {
//   name: string;
//   age: string;
// }
```

Here, we create a new type `StringifiedPerson` where both `name` and `age` are converted to type `string`.

---

# Making Properties Optional

You can use mapped types to **make all properties optional** in an existing type:

```
type Person = {
  name: string;
  age: number;
};

type OptionalPerson = { [K in keyof Person]?: Person[K] };

// Result:
// type OptionalPerson = {
//   name?: string;
//   age?: number;
// }
```

This `OptionalPerson` type has the same properties as `Person`, but **all properties are now optional**.

---

# Making Properties Read-Only

You can also create a mapped type that makes every property **read-only**:

```
type Person = {
  name: string;
  age: number;
};

type ReadOnlyPerson = { readonly [K in keyof Person]: Person[K] };

// Result:
// type ReadOnlyPerson = {
//   readonly name: string;
//   readonly age: number;
// }
```

The `readonly` modifier is applied to all properties of `Person`, making them unmodifiable.

---

# Mapped Types with Conditional Logic

You can use **conditional types** inside a mapped type to apply different transformations based on the property type.

```
type Person = {
  name: string;
  age: number;
  isEmployed: boolean;
};

type PersonWithStringValues = {
  [K in keyof Person]: Person[K] extends boolean ? string : Person[K];
};

// Result:
// type PersonWithStringValues = {
//   name: string;
//   age: number;
//   isEmployed: string;
// }
```

Here:

* If the property type is `boolean`, we change it to `string`.

* If it’s any other type (like `string` or `number`), it remains unchanged.

---

# Using Mapped Types with `as` for Renaming Properties

You can also use mapped types to **rename properties**:

```
type Person = {
  name: string;
  age: number;
};

type RenamedPerson = {
  [K in keyof Person as `new_${K}`]: Person[K];
};

// Result:
// type RenamedPerson = {
//   new_name: string;
//   new_age: number;
// }
```

In this case, we use `as` to prepend `"new_"` to every property name.

---

# Summary

Mapped types allow you to:

1. **Transform** the properties of an existing type.

2. Make properties optional, read-only, or even change their types.

3. Use **conditional logic** within the mapped type to apply transformations based on the property type.

4. Rename properties using the `as` keyword.

Here’s a recap of the most common patterns for mapped types:

| Pattern | Example | Result |
| ----- | ----- | ----- |
| Make properties optional | `{ [K in keyof T]?: T[K] }` | All properties are optional |
| Make properties read-only | `{ readonly [K in keyof T]: T[K] }` | All properties are read-only |
| Change property types | `{ [K in keyof T]: string }` | All properties have type `string` |
| Conditionally change types | `{ [K in keyof T]: T[K] extends boolean ? string : T[K] }` | Change `boolean` properties to `string` |
| Rename properties | `{ [K in keyof T as` new\_${K}`]: T[K] }` | Rename properties by adding a prefix |

Mapped types are incredibly powerful for transforming and manipulating types, allowing you to create flexible and reusable type logic.

Let me know if you need further clarification or more examples\!

