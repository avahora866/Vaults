---
tags:
  - FORMAT
  - C
---
Absolutely\! Let’s dive into these important CMake commands and explain how they work and how to use them effectively.

---

# Create Executables with `add_executable(target_name sources...)`

The `add_executable()` command is used to create an **executable** target in CMake. An **executable** is a program that can be run after it has been compiled (like a `.exe` file on Windows or a binary on Linux/macOS).

## Syntax:

```
add_executable(target_name sources...)
```

*   
  `target_name`: The name of the executable.

* `sources`: A list of source files (typically `.cpp`, `.c`, `.m`, etc.) that make up the executable.

**Example**:

```
add_executable(MyApp main.cpp utils.cpp)
```

In this case:

* `MyApp` is the name of the executable.

* `main.cpp` and `utils.cpp` are the source files that will be compiled to create the executable.

After running CMake and building, you’ll get an executable named `MyApp` in the build directory (or `MyApp.exe` on Windows).

---

# Create Libraries with `add_library()` (STATIC, SHARED, or OBJECT)

The `add_library()` command is used to create a **library target** in CMake. Libraries are collections of compiled code that can be linked to other targets. There are three types of libraries you can create:

1. **STATIC**: A static library is a set of compiled object files that are bundled into one file. This library is linked directly into executables at build time.

2. **SHARED**: A shared library (also known as a **dynamic** or **DLL** library) is linked at runtime, meaning executables that depend on the shared library will load it when the program runs.

3. **OBJECT**: An object library contains compiled object files (but no final library). It’s not linked directly; instead, it is used to combine object files into other targets.

## Syntax:

```
add_library(target_name [STATIC|SHARED|OBJECT] sources...)
```

*   
  `target_name`: The name of the library.

* `[STATIC|SHARED|OBJECT]`: The type of the library (STATIC, SHARED, or OBJECT).

* `sources`: The source files (e.g., `.cpp`, `.c`) that make up the library.

**Examples**:

* **Static Library**:

```
add_library(MyStaticLib STATIC mylib.cpp mylib.h)
```

*   
  This creates a static library called `MyStaticLib`.

* **Shared Library**:

```
add_library(MySharedLib SHARED mylib.cpp mylib.h)
```

*   
  This creates a shared library called `MySharedLib`.

* **Object Library**:

```
add_library(MyObjectLib OBJECT mylib.cpp mylib.h)
```

*   
  This creates an object library called `MyObjectLib`, which compiles the object files but doesn’t link them into a library.

---

# Link Targets Using `target_link_libraries()`

The `target_link_libraries()` command is used to **link libraries or other targets** to your target. This is an essential part of building projects with dependencies between executables and libraries.

## Syntax:

```
target_link_libraries(target_name library_name...)
```

*   
  `target_name`: The name of the target (executable or library) that you want to link to other libraries or targets.

* `library_name`: The name(s) of the library(ies) or target(s) that the target should be linked to.

You can specify **private**, **public**, or **interface** visibility to control how the libraries are propagated.

**Example**:

```
add_executable(MyApp main.cpp)
add_library(MyLib STATIC mylib.cpp)

target_link_libraries(MyApp PRIVATE MyLib)
```

In this case:

* `MyApp` is linked to `MyLib`.

* The `PRIVATE` keyword indicates that the linking is internal to `MyApp` and won’t be propagated to other targets that link to `MyApp`.

You can also link multiple libraries or targets:

```
target_link_libraries(MyApp PUBLIC MyLib OtherLib)
```

Here:

* `MyApp` will link to both `MyLib` and `OtherLib`, and these libraries will be propagated to other targets that link to `MyApp`.

---

# Use `target_include_directories()` for Header Search Paths

The `target_include_directories()` command is used to specify **header search paths** for a target. This tells CMake where to look for header files when compiling the source code.

You can specify whether the include directories are **private**, **public**, or **interface**, similar to how `target_link_libraries()` works.

## Syntax:

```
target_include_directories(target_name [PRIVATE|PUBLIC|INTERFACE] directories...)
```

*   
  `target_name`: The target (either executable or library) for which you want to set the include directories.

* `[PRIVATE|PUBLIC|INTERFACE]`: This specifies whether the directories apply to the target itself, propagate to dependent targets, or are only for dependent targets.

* `directories`: A list of directories where the compiler should look for header files.

**Examples**:

* **Private Include Directories** (only for the target itself):

```
target_include_directories(MyApp PRIVATE ${CMAKE_SOURCE_DIR}/include)
```

*   
  This means `MyApp` will include headers from `${CMAKE_SOURCE_DIR}/include`, but other targets that link to `MyApp` won’t inherit this include path.

* **Public Include Directories** (propagates to dependent targets):

```
target_include_directories(MyLib PUBLIC ${CMAKE_SOURCE_DIR}/include)
```

*   
  This means that both `MyLib` and any target that links to `MyLib` will have `${CMAKE_SOURCE_DIR}/include` added to their header search paths.

* **Interface Include Directories** (only for dependent targets):

```
target_include_directories(MyHeaderOnlyLib INTERFACE ${CMAKE_SOURCE_DIR}/include)
```

*   
  This means that `MyHeaderOnlyLib` is a header-only library, so the include path will only apply to targets that link to `MyHeaderOnlyLib`.

---

# Putting It All Together — Full Example

Here’s a more complete example showing all the concepts in action:

```
cmake_minimum_required(VERSION 3.16)
project(MyProject)

 Create a static library
add_library(MyLib STATIC mylib.cpp)

 Create an executable
add_executable(MyApp main.cpp)

 Link the executable to the library
target_link_libraries(MyApp PRIVATE MyLib)

 Specify include directories for the executable
target_include_directories(MyApp PRIVATE ${CMAKE_SOURCE_DIR}/include)

 Specify include directories for the library
target_include_directories(MyLib PUBLIC ${CMAKE_SOURCE_DIR}/include)
```

In this example:

* `MyLib` is a static library created from `mylib.cpp`.

* `MyApp` is an executable created from `main.cpp`.

* `MyApp` is linked to `MyLib` using `target_link_libraries()`.

* `MyApp` gets include directories privately, while `MyLib` propagates its include directories to any target that links to it.

---

# Key Takeaways

* **`add_executable()`** creates an executable target.

* **`add_library()`** creates a library target (either static, shared, or object).

* **`target_link_libraries()`** links libraries or other targets to your target (either executables or libraries).

* **`target_include_directories()`** sets the header search paths for a target, with different levels of visibility: `PRIVATE`, `PUBLIC`, and `INTERFACE`.

These commands are fundamental to how you define and manage dependencies, target properties, and how CMake handles the build process for your project.

Let me know if you’d like more detailed examples or explanations on any of these\!

