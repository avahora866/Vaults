---
tags:
  - FORMAT
  - C
---
Sure\! Here's a straightforward explanation of what CMake is and why it's used:

# What is CMake?

CMake is a **build system generator**. That means it helps developers manage the process of compiling and linking their code into executable programs or libraries. CMake itself doesn't build software directly; instead, it generates files for other build systems (like Makefiles on Linux or Visual Studio project files on Windows).

# Why is CMake used?

1. **Cross-platform support**:  
    CMake works on many operating systems, including Windows, macOS, and Linux. You can write one set of CMake configuration files, and it will generate platform-specific build files for you.

2. **Abstracts away platform differences**:  
    Instead of writing different build scripts for each platform, you define your project’s structure in a single `CMakeLists.txt` file. CMake handles the details of generating the appropriate build instructions for each environment.

3. **Flexible and powerful**:  
    It supports complex project setups, including dependencies, custom build steps, and external libraries.

4. **Widely adopted**:  
    Many open-source and professional software projects use CMake because it integrates well with popular development tools and compilers.

5. **Out-of-source builds**:  
    CMake encourages keeping build files separate from source code, which helps keep your project directory clean.

# Example:

Instead of writing separate build files for Visual Studio and Make, you just write a `CMakeLists.txt`, and then run:

* On Linux: `cmake . && make`

* On Windows: `cmake . -G "Visual Studio 17 2022"`

CMake figures out the right way to build for your platform and toolset.

Let me know if you'd like a quick example of a `CMakeLists.txt` file.

