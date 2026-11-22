---
tags:
  - FORMAT
  - Typescript
---
# Compiling TypeScript (`tsc file.ts`)

TypeScript code is not directly executed by browsers or Node.js. It must first be compiled (or transpiled) into JavaScript using the **TypeScript Compiler (`tsc`)**.

---

# Steps to Compile TypeScript

## Install TypeScript (if not installed)

Before using the `tsc` command, ensure TypeScript is installed globally or locally in your project.

**Install TypeScript globally:**

```
npm install -g typescript
```

**Install TypeScript in a project:**

```
npm install --save-dev typescript
```

## Create a TypeScript File (`file.ts`)

Example TypeScript code (`example.ts`):

```
let message: string = "Hello, TypeScript!";
console.log(message);
```

## Compile TypeScript to JavaScript

Run the following command in the terminal:

```
tsc example.ts
```

*   
  This generates a JavaScript file (`example.js`) in the same directory.

## Run the Generated JavaScript

Once compiled, run the JavaScript file using Node.js:

```
node example.js
```

Output:

```
Hello, TypeScript!
```

---

# Using a TypeScript Configuration File (`tsconfig.json`)

Instead of compiling individual files, you can configure TypeScript using `tsconfig.json`:

## Create `tsconfig.json`

Run:

```
tsc --init
```

This generates a `tsconfig.json` file where you can set compiler options.

Example `tsconfig.json`:

```
{
  "compilerOptions": {
    "target": "ES6",
    "outDir": "dist",
    "strict": true
  }
}
```

*   
  `target: "ES6"` – Compiles TypeScript to ES6 JavaScript.

* `outDir: "dist"` – Outputs compiled files into the `dist/` folder.

* `strict: true` – Enables strict type checking.

## Compile All TypeScript Files

Run:

```
tsc
```

*   
  This compiles all `.ts` files in the project according to `tsconfig.json`.

---

# Watching for File Changes (`tsc --watch`)

To automatically recompile files when changes are made, use:

```
tsc --watch
```

---

# Summary

* Use `tsc file.ts` to compile a TypeScript file into JavaScript.

* Configure `tsconfig.json` for project-wide settings.

* Use `tsc --watch` to automatically recompile on file changes.

Would you like help setting up a TypeScript project?

