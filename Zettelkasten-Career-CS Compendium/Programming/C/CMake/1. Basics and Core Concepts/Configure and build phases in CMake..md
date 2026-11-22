---
tags:
  - FORMAT
  - C
---
Absolutely. Here's a comprehensive explanation of the **configure** and **build** phases in CMake, and how they work together in a typical development workflow.

---

# Overview of CMake Workflow

CMake separates the build process into two distinct phases:

1. **Configure Phase**

2. **Build Phase**

Each serves a different purpose in turning your source code into a working program.

---

# Configure Phase

**Goal**: Generate build system files (e.g., Makefiles, Ninja files, Visual Studio project files)

**What happens**:

* CMake reads your `CMakeLists.txt` file.

* It detects your platform, compiler, and available system libraries.

* It resolves configuration options (like build type, paths, features).

* It checks dependencies, compiler flags, and toolchain information.

* It generates native build files tailored to the build tool you selected.

**Example commands**:

```
cmake -S . -B build
```

*   
  `-S .` means the source directory is the current folder.

* `-B build` means CMake will generate the build system into a new folder named `build`.

After this step, the `build` folder will contain files like `Makefile` or `build.ninja` (depending on the generator you chose).

**Key concepts**:

* **Out-of-source builds**: Keeps your build files separate from source files.

* **CMake cache**: CMake stores configuration settings in `CMakeCache.txt`, so you don’t need to reconfigure everything every time unless something changes.

---

# Build Phase

**Goal**: Actually compile and link your code using the generated build files.

**What happens**:

* The selected build tool (Make, Ninja, MSBuild, etc.) is invoked using the instructions generated in the configure phase.

* It compiles source files, links them into libraries or executables, and puts them in the output directory.

**Example command**:

```
cmake --build build
```

This tells CMake to run the native build tool in the `build` folder.

You can also specify configuration options (like Debug or Release) for multi-config generators (such as Visual Studio):

```
cmake --build build --config Release
```

---

# Summary of Phases

| Phase | Command | What It Does |
| ----- | ----- | ----- |
| Configure | `cmake -S . -B build` | Generates build files |
| Build | `cmake --build build` | Compiles and links code |

---

# Bonus: Clean Builds

If things go wrong or you want a fresh build:

```
rm -rf build
cmake -S . -B build
cmake --build build
```

This ensures everything is rebuilt from scratch.

---

Let me know if you’d like to go over what happens inside `CMakeLists.txt` during the configure phase.

