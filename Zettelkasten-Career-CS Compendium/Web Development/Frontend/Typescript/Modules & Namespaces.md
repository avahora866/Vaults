---
tags:
  - FORMAT
  - Typescript
---
# Importing and Exporting Modules in TypeScript

TypeScript supports **modules**, which allow you to organize your code into separate files. This makes your code more maintainable and reusable. Modules in TypeScript are based on **ECMAScript 6 (ES6) modules**. You can **import** and **export** functions, classes, objects, and variables between different files.

Let’s go over how you can import and export modules, use default exports, and work with namespaces in TypeScript.

---

# Importing and Exporting Modules

## Exporting

To make parts of a file accessible to other files, you need to **export** them. You can export variables, functions, classes, interfaces, and types.

There are two main types of exports in TypeScript: **named exports** and **default exports**.

### Named Exports

A **named export** allows you to export multiple items from a module and import them by their exact names in another module.

**Example of named exports**:

```
// math.ts

export function add(x: number, y: number): number {
  return x + y;
}

export function subtract(x: number, y: number): number {
  return x - y;
}
```

Here, we have two functions `add` and `subtract` that are exported from the `math.ts` file.

## Importing Named Exports

When you import named exports, you need to use the exact name of the exported item.

```
// main.ts

import { add, subtract } from "./math";

console.log(add(2, 3)); // Output: 5
console.log(subtract(5, 3)); // Output: 2
```

*   
  You use the `{}` syntax to import the named exports.

* You specify the file path (relative or absolute) in the import statement.

### Default Export

A **default export** allows you to export a single entity from a file. It can be a function, class, object, or any other value.

**Example of default export**:

```
// greet.ts

export default function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

Here, we are exporting the `greet` function as the default export from the `greet.ts` file.

## Importing Default Exports

When importing a **default export**, you don’t need to use curly braces and can give the imported entity any name you like.

```
// main.ts

import greet from "./greet";

console.log(greet("Alice")); // Output: Hello, Alice!
```

*   
  You can name the import anything you want, unlike named imports, where the name must match the exported name.

## Combining Named and Default Exports

You can mix **default exports** and **named exports** in the same file.

```
// utils.ts

export default function greet(name: string): string {
  return `Hello, ${name}!`;
}

export function add(x: number, y: number): number {
  return x + y;
}
```

You can import both default and named exports together:

```
// main.ts

import greet, { add } from "./utils";

console.log(greet("Alice")); // Output: Hello, Alice!
console.log(add(2, 3)); // Output: 5
```

# Using Namespaces in TypeScript

Namespaces in TypeScript provide a way to group related code under a single name. They allow you to logically organize code and avoid naming conflicts. While ES6 modules are now the standard for module organization, namespaces can still be useful for organizing code within a single file.

## Creating a Namespace

You can define a **namespace** using the `namespace` keyword. A namespace can contain functions, variables, classes, and interfaces.

**Example of a namespace**:

```
// shapes.ts

namespace Shapes {
  export class Circle {
    constructor(public radius: number) {}
    
    getArea(): number {
      return Math.PI * this.radius ** 2;
    }
  }

  export class Square {
    constructor(public sideLength: number) {}

    getArea(): number {
      return this.sideLength ** 2;
    }
  }
}
```

*   
  In this example, `Shapes` is a namespace that contains two classes: `Circle` and `Square`.

* The `export` keyword is used to expose the classes and their members, making them accessible outside the namespace.

## Using a Namespace

To use a class or function inside a namespace, you refer to the namespace name, followed by the class or function name.

```
// main.ts

/// <reference path="shapes.ts" /> // Include the file with the namespace

const circle = new Shapes.Circle(5);
console.log(circle.getArea()); // Output: 78.53981633974483

const square = new Shapes.Square(4);
console.log(square.getArea()); // Output: 16
```

*   
  The `/// <reference path="..." />` directive is used to include the file containing the namespace when both files are in the same project (typically in TypeScript projects using namespaces).

* We can access the `Circle` and `Square` classes using `Shapes.Circle` and `Shapes.Square`.

## Namespace and Modules Together

Namespaces are often used in older code or when working in TypeScript files that don’t use ES6 module syntax. However, if you're using **ES6 modules**, you usually don't need namespaces because you can rely on import/export to organize your code.

Here's an example where namespaces can still be useful:

```
namespace Utils {
  export function multiply(x: number, y: number): number {
    return x * y;
  }

  export function divide(x: number, y: number): number {
    return x / y;
  }
}

// Usage of the namespace
console.log(Utils.multiply(2, 3)); // Output: 6
console.log(Utils.divide(6, 3));   // Output: 2
```

## Combining Namespaces and Modules

In a larger TypeScript project, you may combine both namespaces and modules to organize your code. A **namespace** can be used within a **module**, and a **module** can be nested within a namespace if needed.

```
namespace MyNamespace {
  export class MyClass {
    sayHello() {
      return "Hello from MyClass!";
    }
  }

  export module MyModule {
    export function myFunction() {
      return "Hello from MyModule!";
    }
  }
}

const myClassInstance = new MyNamespace.MyClass();
console.log(myClassInstance.sayHello()); // Output: Hello from MyClass!

console.log(MyNamespace.MyModule.myFunction()); // Output: Hello from MyModule!
```

In this example:

* `MyClass` is a class inside a namespace `MyNamespace`.

* `MyModule` is an internal module inside the namespace, and we can access its functions using `MyNamespace.MyModule`.

# Summary:

* **Exporting and Importing Modules**:

  * **Named exports**: Allows multiple named exports from a module. These exports must be imported by name.

  * **Default export**: Allows a single default export. You can import it with any name.

* **Using Namespaces**:

  * A **namespace** helps you group related functionality in a single unit. It's useful for organizing code and avoiding naming conflicts.

  * You can use `export` to make parts of a namespace accessible outside its scope.

## When to Use:

* **Modules**: Prefer using **modules** (`import`/`export`) in modern TypeScript projects. They help with better code organization, are compatible with most build systems, and are the standard in ES6 and beyond.

* **Namespaces**: Use **namespaces** for older code or when working in environments where modules are not supported. They are mostly used for organizing code within a single file or set of related files.

