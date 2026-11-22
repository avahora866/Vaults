---
tags:
  - FORMAT
  - C
---
Definitely — let's break down the difference between **CMake commands**, **variables**, and **modules** in a simple, clear way:

---

# CMake Commands

**What are they?**  
 Commands are **built-in functions** that tell CMake what to do. They define how to configure, build, and manage your project.

**Think of them as instructions** in your `CMakeLists.txt`.

**Examples**:

```
project(MyProject)
add_executable(MyApp main.cpp)
set(MY_VAR "value")
```

**Common commands**:

* `project()` – sets up your project's name and version

* `add_executable()` – defines an executable to build

* `add_library()` – defines a library

* `set()` – defines or changes variables

* `target_link_libraries()` – links libraries to targets

---

# CMake Variables

**What are they?**  
 Variables **store data** like file paths, settings, options, or flags. They let you control how CMake configures the build.

**Think of them as settings** or pieces of information.

**Examples**:

```
set(CMAKE_CXX_STANDARD 17)
set(SOURCE_FILES main.cpp utils.cpp)
```

**Common built-in variables**:

* `CMAKE_CXX_STANDARD` – sets the C++ version

* `CMAKE_BUILD_TYPE` – sets build type (e.g., Debug or Release)

* `CMAKE_SOURCE_DIR` – path to the root source directory

You can create your own variables, too:

```
set(MY_NAME "ChatGPT")
message(STATUS "Name is ${MY_NAME}")
```

---

# CMake Modules

**What are they?**  
 Modules are **reusable `.cmake` files** that CMake can load to provide extra functionality — like finding external libraries or setting up features.

**Think of them as plugins** or reusable code snippets.

**Examples**:

```
include(GNUInstallDirs)
find_package(OpenGL REQUIRED)
```

*   
  `include()` loads a module or script file.

* `find_package()` searches for installed packages and loads the right module (like `FindOpenGL.cmake`).

Modules can:

* Set variables

* Define functions/commands

* Help locate and link external software (like Boost, OpenSSL, etc.)

---

# Quick Summary

| Concept | Purpose | Example |
| ----- | ----- | ----- |
| Command | Tells CMake what to do | `add_executable(MyApp main.cpp)` |
| Variable | Stores settings or information | `set(CMAKE_CXX_STANDARD 17)` |
| Module | Loads reusable functionality or scripts | `find_package(OpenGL REQUIRED)` |

---

Let me know if you want a working example that uses all three — commands, variables, and modules — in one `CMakeLists.txt`.

