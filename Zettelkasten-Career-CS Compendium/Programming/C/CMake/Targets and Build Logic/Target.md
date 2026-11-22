---
tags:
  - FORMAT
  - C
---
Certainly\! In CMake, the term **target** refers to an entity that you want to build — like an **executable**, a **library**, or a **custom command**. Targets are the core components of a CMake build system, and they represent the outputs that will be created as a result of the build process.

Let’s break it down in more detail:

---

# What is a Target?

A **target** is an object in the build process that defines a specific output file (such as an executable or a library) and the set of instructions (such as source files, dependencies, and build settings) needed to create that output. Targets are the primary building blocks when working with CMake.

## Common Types of Targets:

1. **Executable Targets** – These are compiled programs that you run (e.g., `.exe` files on Windows, or binaries on Unix-like systems).

2. **Library Targets** – These are compiled libraries (e.g., `.lib`, `.a`, `.dll`, `.so` files) that you can link against in other projects.

3. **Custom Targets** – These are user-defined tasks that can be used for custom build actions, such as generating files or running scripts.

---

# Executable Target

An **executable target** is a program that can be run once built. It’s typically a `.exe` file (on Windows) or an executable binary (on Linux/macOS).

**How to define it**:

```
add_executable(MyApp main.cpp utils.cpp)
```

In this example:

* `add_executable()` creates a target named `MyApp`.

* It compiles the source files (`main.cpp`, `utils.cpp`) into an executable.

Once built, the output will be the executable `MyApp` (or `MyApp.exe` on Windows).

---

# Library Target

A **library target** is a collection of code that can be linked to other programs or libraries. There are two types of libraries: **static** and **shared (dynamic)**.

* **Static libraries** (`.a`, `.lib`): Compiled code that is included directly into the final executable at build time.

* **Shared libraries** (`.so`, `.dll`): Compiled code that is linked to at runtime (shared across multiple programs).

**How to define a static library**:

```
add_library(MyLib STATIC mylib.cpp mylib.h)
```

**How to define a shared library**:

```
add_library(MyLib SHARED mylib.cpp mylib.h)
```

In these examples:

* `add_library()` creates a library target named `MyLib`.

* `STATIC` or `SHARED` specifies the type of library.

* `mylib.cpp` and `mylib.h` are the source files that make up the library.

A library target is useful when you want to reuse code in multiple projects or modules.

---

# Custom Targets

A **custom target** allows you to define any kind of custom build step or task that doesn’t directly produce a file like an executable or library. It’s useful for actions like running scripts, copying files, or generating documentation.

**How to define a custom target**:

```
add_custom_target(MyCustomTarget
    COMMAND python generate_docs.py
    COMMENT "Generating documentation"
)
```

In this example:

* `add_custom_target()` creates a target named `MyCustomTarget`.

* The target runs a Python script (`generate_docs.py`) when the target is built.

* The `COMMENT` provides a description that is shown when the target is built.

---

# Example: Combining Executable, Library, and Custom Target

Here’s an example of a more complex CMake setup:

```
cmake_minimum_required(VERSION 3.16)
project(MyProject)

 Define a library target
add_library(MyLib STATIC mylib.cpp)

 Define an executable target
add_executable(MyApp main.cpp)

 Link the library to the executable
target_link_libraries(MyApp PRIVATE MyLib)

 Define a custom target for generating documentation
add_custom_target(GenerateDocs
    COMMAND python generate_docs.py
    COMMENT "Generating documentation"
)

 Add the custom target as a dependency of the executable
add_dependencies(MyApp GenerateDocs)
```

In this example:

* `MyLib` is a static library target.

* `MyApp` is an executable target that links to `MyLib`.

* `GenerateDocs` is a custom target that generates documentation, and it’s run before `MyApp` is built because of the `add_dependencies()` command.

---

# Summary

| Target Type | Purpose | Example Command |
| ----- | ----- | ----- |
| **Executable** | A program you can run after building. | `add_executable(MyApp main.cpp)` |
| **Library** | A set of reusable code (can be static or shared). | `add_library(MyLib STATIC mylib.cpp)` |
| **Custom** | A user-defined task (e.g., running a script, generating files). | `add_custom_target(MyTarget COMMAND python script.py)` |

---

# Key Concepts:

* **Target**: An output that CMake will build (e.g., an executable or a library).

* **Dependencies**: You can link targets to each other, meaning one target might depend on another to be built first.

* **Linking**: Targets can link to other targets (e.g., executables linking to libraries).

Let me know if you need any more details or examples\!

