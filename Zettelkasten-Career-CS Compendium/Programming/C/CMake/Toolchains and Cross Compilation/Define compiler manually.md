---
tags:
  - FORMAT
  - C
---
# Defining Compilers Manually with `CMAKE_C_COMPILER` and `CMAKE_CXX_COMPILER`

In CMake, the variables `CMAKE_C_COMPILER` and `CMAKE_CXX_COMPILER` allow you to manually specify the C and C++ compilers to be used during the configuration and build process. This is especially useful if you need to use a custom compiler or toolchain instead of the default system compilers detected by CMake.

By default, CMake tries to detect the C and C++ compilers based on the system's environment. However, in certain scenarios, such as using a non-standard compiler, a cross-compilation setup, or a specific version of a compiler, you might need to manually specify which compilers should be used.

---

# How to Define Compilers Manually

You can define the compilers for **C** and **C++** using `CMAKE_C_COMPILER` and `CMAKE_CXX_COMPILER` in the CMake configuration process. These variables specify the paths to the **C** and **C++** compilers that should be used by CMake.

Here’s how you can manually specify the compilers:

## Example: Specifying Compilers in the Command Line

You can specify the compilers directly when running the `cmake` command by passing the variables using the `-D` flag:

```
cmake -DCMAKE_C_COMPILER=/path/to/gcc -DCMAKE_CXX_COMPILER=/path/to/g++ ..
```

*   
  `CMAKE_C_COMPILER=/path/to/gcc`: Specifies the path to the **C** compiler (`gcc`).

* `CMAKE_CXX_COMPILER=/path/to/g++`: Specifies the path to the **C++** compiler (`g++`).

This tells CMake to use the specified compilers instead of the default ones it detects from the system.

## Example: Specifying Compilers in `CMakeLists.txt`

You can also specify the compilers directly in your `CMakeLists.txt` file (although this is less common). This method is typically used for specific configurations or for projects that need to be shared across multiple users who may not want to set environment variables themselves.

```
set(CMAKE_C_COMPILER "/path/to/gcc")
set(CMAKE_CXX_COMPILER "/path/to/g++")
```

*   
  `set(CMAKE_C_COMPILER "/path/to/gcc")`: This sets the **C** compiler to the specified path (`gcc`).

* `set(CMAKE_CXX_COMPILER "/path/to/g++")`: This sets the **C++** compiler to the specified path (`g++`).

This will force CMake to use the specified compilers when configuring and building the project.

## Example: Using a Toolchain File

For cross-compilation or complex build environments, you may define compilers in a **toolchain file**. This method is particularly useful when working on projects targeting different platforms or architectures.

Here’s an example of a simple toolchain file (`toolchain.cmake`) that defines custom compilers:

```
 toolchain.cmake
set(CMAKE_C_COMPILER /path/to/your/custom/gcc)
set(CMAKE_CXX_COMPILER /path/to/your/custom/g++)
```

To use this toolchain file during the configuration step, pass it with the `-DCMAKE_TOOLCHAIN_FILE` option:

```
cmake -DCMAKE_TOOLCHAIN_FILE=toolchain.cmake ..
```

This will configure the project using the compilers defined in the toolchain file.

---

# Why Manually Set `CMAKE_C_COMPILER` and `CMAKE_CXX_COMPILER`?

1. **Cross-compilation**: If you are cross-compiling for a platform that is different from your host machine, you will likely need to specify compilers for the target platform (e.g., using a cross-compiler like `arm-linux-gnueabi-gcc` to build for ARM when working on an x86 machine).

2. **Custom Compilers**: If you want to use a non-default or non-standard compiler, you can specify the exact version or a specific compiler. For example, if you want to use a specific version of GCC or Clang that is not the default system compiler.

3. **Toolchain Management**: When building with a specific toolchain (for example, an embedded system's toolchain), manually setting these compilers ensures that CMake uses the correct tools to compile your project, rather than relying on the system's default settings.

4. **Testing and Debugging**: If you need to test your project with a different compiler for performance or compatibility reasons, manually setting the compilers allows you to quickly switch between them.

---

# Example Use Case

Suppose you are working on a project and want to build it using **Clang** instead of the default GCC compiler on a Linux system. You can specify the compilers as follows:

```
cmake -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ ..
```

This will configure the project to use **Clang** for both C and C++ compilation instead of the default **GCC**.

If you are using **cross-compilation** to target an embedded system, you might do something like:

```
cmake -DCMAKE_C_COMPILER=/path/to/arm-linux-gnueabi-gcc -DCMAKE_CXX_COMPILER=/path/to/arm-linux-gnueabi-g++ ..
```

In this case, you're specifying the cross-compilers needed to build for an ARM platform from an x86 host machine.

---

# Summary

* **`CMAKE_C_COMPILER`** and **`CMAKE_CXX_COMPILER`** are CMake variables used to specify the **C** and **C++** compilers for the build process.

* You can set these variables manually either through the **command line** (`-DCMAKE_C_COMPILER` and `-DCMAKE_CXX_COMPILER`), within your `CMakeLists.txt` file, or through a **toolchain file**.

* Manually setting the compilers is especially useful for **cross-compilation**, using **custom compilers**, or when you need to specify a particular **toolchain**.

* This feature is particularly helpful in environments where you are working on multiple platforms or need to ensure consistent toolchain usage across different build systems or machines.

By manually specifying compilers, you have full control over the tools used to build your project, which can be crucial for targeting different platforms or custom build environments.

