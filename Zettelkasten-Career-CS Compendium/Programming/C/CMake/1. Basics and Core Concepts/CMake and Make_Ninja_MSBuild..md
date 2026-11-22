---
tags:
  - FORMAT
  - C
---
Absolutely. Here's a clear breakdown of the difference between **CMake** and tools like **Make**, **Ninja**, and **MSBuild**.

# CMake vs Make/Ninja/MSBuild

## CMake

**Type**: Build **system generator**

* CMake is **not** a build tool itself.

* It **generates** build files for other tools like Make, Ninja, or MSBuild.

* You write a `CMakeLists.txt` file to define your project.

* Then, CMake processes that file and creates build files appropriate for your platform and toolchain.

Think of CMake as a **translator** that turns a project description into build instructions for your chosen build tool.

## Make, Ninja, MSBuild

**Type**: Actual **build tools**

These tools are responsible for **executing** the build: compiling code, linking files, and producing output like executables or libraries.

* **Make**:  
   One of the oldest build tools, often used on Unix/Linux systems. It uses Makefiles. It's not the fastest, especially for large projects.

* **Ninja**:  
   A newer, high-performance build tool. It’s very fast and ideal for large projects. CMake can generate Ninja build files if requested.

* **MSBuild**:  
   Microsoft's build system, used with Visual Studio on Windows. It understands `.sln` and `.vcxproj` files.

# Summary

| Tool | Type | Role |
| ----- | ----- | ----- |
| CMake | Build system generator | Generates build files |
| Make | Build tool | Compiles code using Makefiles |
| Ninja | Build tool | Fast compilation using Ninja files |
| MSBuild | Build tool | Builds Visual Studio projects |

So:

* **CMake** prepares the plan.

* **Make**, **Ninja**, or **MSBuild** execute the plan.

Let me know if you'd like an example workflow using CMake with one of these tools.

