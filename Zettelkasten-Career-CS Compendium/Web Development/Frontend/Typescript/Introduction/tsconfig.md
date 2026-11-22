---
tags:
  - FORMAT
  - Typescript
---
# Understanding the `tsconfig.json` File 📝

The `tsconfig.json` file is a **configuration file** used by the TypeScript compiler (`tsc`). It defines how TypeScript should process your project files and specifies compiler options, including how to generate the JavaScript output and any rules or settings to enforce during the compilation process.

---

# Structure of `tsconfig.json`

The `tsconfig.json` file is written in **JSON** format, and it typically contains the following key properties:

---

# `compilerOptions`

The `compilerOptions` section defines the settings for the TypeScript compiler. Some commonly used options include:

* **`target`**: Specifies the version of JavaScript to which TypeScript should compile. Common values are:

  * `ES5` (default)

  * `ES6` (or `ES2015`), `ES2016`, `ES2020`, etc.

  * `ESNext` (latest ECMAScript version)

Example:

```
"target": "ES5"
```

*   
  **`module`**: Defines the module system TypeScript should use (e.g., `CommonJS`, `ES6`, `AMD`, `System`).

Example:

```
"module": "ES6"
```

*   
  **`outDir`**: Specifies the output directory for the compiled JavaScript files.

Example:

```
"outDir": "./dist"
```

*   
  **`rootDir`**: Specifies the root directory of your source TypeScript files.

Example:

```
"rootDir": "./src"
```

*   
  **`strict`**: Enables all strict type-checking options (e.g., `noImplicitAny`, `strictNullChecks`, etc.).

Example:

```
"strict": true
```

*   
  **`esModuleInterop`**: Ensures compatibility between CommonJS and ES Modules, allowing `import` of CommonJS modules.

Example:

```
"esModuleInterop": true
```

*   
  **`sourceMap`**: Tells TypeScript to generate source maps for debugging, which map the compiled JavaScript back to the original TypeScript.

Example:

```
"sourceMap": true
```

*   
  **`skipLibCheck`**: Skips type checking of declaration files (`.d.ts`), speeding up the build process (useful for large projects).

Example:

```
"skipLibCheck": true
```

---

# `include`

The `include` array specifies the files or directories to be included in the compilation process. It is usually used to include TypeScript files or specific directories.

Example:

```
"include": [
  "src/**/*.ts",
  "src/**/*.tsx"
]
```

This example will include all `.ts` and `.tsx` files in the `src` directory and its subdirectories.

---

# `exclude`

The `exclude` array specifies files or directories that should **not** be included in the compilation process. Commonly, you'd exclude `node_modules` or build directories.

Example:

```
"exclude": [
  "node_modules",
  "dist"
]
```

This will exclude `node_modules` and `dist` directories from being processed.

---

# `files`

Unlike `include`, the `files` array explicitly lists the individual files you want to compile. This is useful if you want to compile specific files rather than a directory or a pattern.

Example:

```
"files": [
  "src/main.ts",
  "src/app.ts"
]
```

---

# `extends`

The `extends` field allows you to inherit settings from another `tsconfig.json` file. This is useful for extending base configurations in larger projects or libraries.

Example:

```
"extends": "./base-config.json"
```

This will include the settings from the `base-config.json` file.

---

# `references`

The `references` field is used for **project references**. It allows one TypeScript project to reference another, enabling you to build and manage larger TypeScript projects.

Example:

```
"references": [
  { "path": "./libs/lib1" },
  { "path": "./libs/lib2" }
]
```

---

# `typeRoots` and `types`

These options control where TypeScript looks for **type declaration files** (`.d.ts` files).

* **`typeRoots`**: Specifies the directories to search for type definitions (used mainly in Node.js projects).

Example:

```
"typeRoots": ["./node_modules/@types"]
```

*   
  **`types`**: Specifies which types to include, rather than including all types in `node_modules/@types`.

Example:

```
"types": ["node", "jest"]
```

---

# `compileOnSave`

This option allows TypeScript to compile the project when you save a file, which is useful in editors like Visual Studio Code. It is often set to `true` for better editor integration.

Example:

```
"compileOnSave": true
```

---

# `watch`

If you use the `tsc` command with the `--watch` flag, TypeScript will automatically recompile your code when changes are made. The `watch` option is not typically in the `tsconfig.json` but can be specified when running `tsc` directly.

---

# Example `tsconfig.json` File

Here’s a simple example that combines many of the options we’ve discussed:

```
{
  "compilerOptions": {
    "target": "ES6",
    "module": "ES6",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "sourceMap": true,
    "skipLibCheck": true
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "exclude": [
    "node_modules",
    "dist"
  ]
}
```

# Explanation:

* **`compilerOptions`**:

  * Compiles TypeScript to ES6 JavaScript.

  * Enables strict mode for type safety.

  * Outputs the JavaScript files to `./dist`.

  * Generates source maps for easier debugging.

* **`include`** and **`exclude`**:

  * Includes all TypeScript files in `src`.

  * Excludes the `node_modules` and `dist` directories from the compilation.

---

# Conclusion

The `tsconfig.json` file is crucial in defining the behavior of the TypeScript compiler for your project. It allows you to specify the target JavaScript version, set up strict type checking, configure module systems, control where to find and output files, and much more. Properly configuring `tsconfig.json` ensures smooth development and optimized compilation of your TypeScript projects.

