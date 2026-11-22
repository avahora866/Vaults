---
tags:
  - FORMAT
  - Typescript
---
Absolutely\! Here's a clear breakdown of these three topics:

---

# Using `try...catch` in TypeScript

Just like in JavaScript, TypeScript uses `try...catch` to handle **runtime errors** safely.

## Syntax

```
try {
  // Code that might throw an error
} catch (error) {
  // Handle the error
}
```

## Example

```
function parseJson(input: string) {
  try {
    return JSON.parse(input);
  } catch (error) {
    console.error("Failed to parse JSON:", error);
    return null;
  }
}
```

## Tip:

By default, the `catch` block variable is typed as `any`, so you might want to explicitly type it if needed:

```
catch (error: unknown) {
  if (error instanceof Error) {
    console.error(error.message);
  }
}
```

Using `unknown` is safer than `any` and encourages you to narrow the type before using it.

---

# Handling Errors with the `never` Type

The `never` type represents values that **should never happen**. It is commonly used in situations like:

* Exhaustive checks in `switch` statements

* Handling unexpected values

## Example: Exhaustiveness Check

```
type Shape = "circle" | "square";

function handleShape(shape: Shape) {
  switch (shape) {
    case "circle":
      console.log("It's a circle");
      break;
    case "square":
      console.log("It's a square");
      break;
    default:
      const _exhaustiveCheck: never = shape; // Error if shape isn't fully handled
      throw new Error(`Unknown shape: ${shape}`);
  }
}
```

This pattern forces you (at compile time) to handle all possible values of a union. If you forget to handle one, TypeScript will give you an error.

---

# Debugging TypeScript in VS Code

VS Code has excellent built-in TypeScript debugging support.

## Step-by-Step Setup

1. **Install `ts-node` or compile with `tsc`**

   * You can run and debug `.ts` files directly with `ts-node`, or compile them using `tsc` and debug the JavaScript.

2. **Create a launch configuration**

   * Open your `.vscode/launch.json` (auto-created by VS Code or via the debug panel)

   * Example config for `ts-node`:

```
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug TypeScript",
      "runtimeArgs": ["-r", "ts-node/register"],
      "args": ["${workspaceFolder}/src/index.ts"],
      "cwd": "${workspaceFolder}"
    }
  ]
}
```

   *   
     Or if using compiled JavaScript:

```
{
  "type": "node",
  "request": "launch",
  "name": "Launch Program",
  "program": "${workspaceFolder}/dist/index.js",
  "outFiles": ["${workspaceFolder}/dist/**/*.js"]
}
```

3.   
   **Set breakpoints** in your `.ts` files

4. **Start debugging** using the Debug panel or `F5`

## Tips

* Make sure source maps are enabled in `tsconfig.json`:

```
{
  "compilerOptions": {
    "sourceMap": true
  }
}
```

*   
  Use `console.log`, `debugger`, or breakpoints to inspect values

---

Let me know if you'd like an example repo setup or help writing a debug config for your specific project.

