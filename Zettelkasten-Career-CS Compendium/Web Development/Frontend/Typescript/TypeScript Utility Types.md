---
tags:
  - FORMAT
  - Typescript
---
Absolutely\! Let’s talk about **TypeScript Utility Types**—they're built-in helpers that allow you to **transform types** in powerful ways without repeating yourself. Think of them like shortcuts for building or reshaping types.

---

# What Are Utility Types?

**Utility types** are predefined generic types provided by TypeScript. They allow you to:

* Make certain properties optional or required

* Create read-only versions of types

* Pick or omit certain properties

* Extract types from other types

They’re defined in TypeScript’s standard library (`lib.d.ts`) and work with your own types or with built-in ones.

---

# Most Common Utility Types (with examples)

---

## `Partial<Type>`

Makes all properties in `Type` optional.

```
type User = {
  name: string;
  age: number;
};

type PartialUser = Partial<User>;
// Equivalent to:
// type PartialUser = { name?: string; age?: number; }
```

---

## `Required<Type>`

Makes all properties in `Type` required.

```
type User = {
  name?: string;
  age?: number;
};

type FullUser = Required<User>;
// All properties must now be defined: { name: string; age: number }
```

---

## `Readonly<Type>`

Makes all properties in `Type` read-only.

```
type User = {
  name: string;
  age: number;
};

type ReadonlyUser = Readonly<User>;
// You can't modify any property now:
// const user: ReadonlyUser = { name: "Alice", age: 30 };
// user.age = 31; // Error
```

---

## `Pick<Type, Keys>`

Creates a new type by **selecting** a subset of properties from `Type`.

```
type User = {
  id: number;
  name: string;
  age: number;
};

type UserPreview = Pick<User, "name" | "age">;
// Result: { name: string; age: number }
```

---

## `Omit<Type, Keys>`

Creates a new type by **removing** specified properties from `Type`.

```
type User = {
  id: number;
  name: string;
  age: number;
};

type UserWithoutID = Omit<User, "id">;
// Result: { name: string; age: number }
```

---

## `Record<Keys, Type>`

Constructs a type with a set of keys `Keys` and values of type `Type`.

```
type Roles = "admin" | "user" | "guest";

type RolePermissions = Record<Roles, boolean>;
// Equivalent to:
// type RolePermissions = {
//   admin: boolean;
//   user: boolean;
//   guest: boolean;
// }
```

---

## `Exclude<UnionType, ExcludedMembers>`

Removes specific members from a union type.

```
type Roles = "admin" | "user" | "guest";
type NonGuest = Exclude<Roles, "guest">;
// Result: "admin" | "user"
```

---

## `Extract<UnionType, Members>`

Opposite of `Exclude`: extracts only the specified members from a union.

```
type Roles = "admin" | "user" | "guest";
type OnlyGuests = Extract<Roles, "guest" | "bot">;
// Result: "guest"
```

---

## `ReturnType<Function>`

Gets the return type of a function.

```
function getUser() {
  return { name: "Alice", age: 30 };
}

type UserReturn = ReturnType<typeof getUser>;
// Result: { name: string; age: number }
```

---

## `Parameters<Function>`

Gets the types of parameters passed to a function as a tuple.

```
function greet(name: string, age: number) {}

type GreetParams = Parameters<typeof greet>;
// Result: [string, number]
```

---

# When Should You Use Utility Types?

* To avoid repeating yourself when manipulating types

* To create reusable, composable type transformations

* To build APIs and data models that are flexible and strongly typed

---

# Summary Table:

| Utility Type | What It Does |
| ----- | ----- |
| `Partial<T>` | Makes all properties optional |
| `Required<T>` | Makes all properties required |
| `Readonly<T>` | Makes all properties read-only |
| `Pick<T, K>` | Picks specific keys from a type |
| `Omit<T, K>` | Removes specific keys from a type |
| `Record<K, T>` | Maps a set of keys to a specific type |
| `Exclude<T, U>` | Removes types from a union |
| `Extract<T, U>` | Extracts types from a union |
| `ReturnType<T>` | Gets return type of a function |
| `Parameters<T>` | Gets parameter types of a function |

---

Want examples for a specific one in a real-world scenario? Let me know\!

