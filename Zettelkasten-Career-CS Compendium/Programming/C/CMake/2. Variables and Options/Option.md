---
tags:
  - FORMAT
  - C
---
Certainly\! In CMake, the `option()` command is used to define **user-configurable toggles** for your build process. These toggles are typically boolean settings (enabled or disabled) that allow users to choose certain features or options when configuring the project. The value of the option can control the inclusion or exclusion of specific features, libraries, or compile-time options.

# Syntax of `option()`

```
option(OPTION_NAME "Description of the option" DEFAULT_VALUE)
```

*   
  **OPTION\_NAME**: The name of the option. This is the name that will be used to reference the option in the CMake configuration and to enable or disable the feature.

* **"Description of the option"**: A short description of what the option does. This description is shown in the output of `cmake` when configuring the project.

* **DEFAULT\_VALUE**: The default value of the option. This is either `ON` (enabled) or `OFF` (disabled).

# Example:

Here’s a basic example that uses `option()` to let users enable or disable a feature, such as enabling a debug mode or an optional library.

```
cmake_minimum_required(VERSION 3.16)
project(MyProject)

 Define an option for enabling/disabling debugging
option(ENABLE_DEBUG "Enable debug mode" OFF)

 Based on the option, add compile definitions or flags
if(ENABLE_DEBUG)
    add_definitions(-DDEBUG_MODE)
    message(STATUS "Debug mode enabled")
else()
    message(STATUS "Debug mode disabled")
endif()

 You can also use this option to conditionally add a library or source files
option(USE_EXTRA_LIBRARY "Enable extra library" ON)
if(USE_EXTRA_LIBRARY)
    add_subdirectory(extra_lib)
    message(STATUS "Extra library enabled")
else()
    message(STATUS "Extra library disabled")
endif()

add_executable(MyApp main.cpp)
```

# Explanation:

* **`option(ENABLE_DEBUG "Enable debug mode" OFF)`**: This defines a user-configurable option called `ENABLE_DEBUG` with a description "Enable debug mode". The default value is `OFF`, meaning debug mode is disabled by default.

* **`if(ENABLE_DEBUG)`**: If `ENABLE_DEBUG` is set to `ON`, CMake will add the `-DDEBUG_MODE` flag to the compiler, enabling debug-specific functionality in the project.

* **`option(USE_EXTRA_LIBRARY "Enable extra library" ON)`**: This defines another option, `USE_EXTRA_LIBRARY`, which is enabled by default (`ON`).

* **`add_subdirectory(extra_lib)`**: If `USE_EXTRA_LIBRARY` is enabled, it adds a subdirectory (`extra_lib`) that contains additional code or libraries.

* **`message(STATUS ...)`**: This prints a message to the user indicating whether the option was enabled or disabled.

# How Users Can Configure Options

When users configure the project using CMake, they can change the value of these options via the CMake GUI or the command line.

## Command Line:

On the command line, users can specify the values of these options when running CMake to configure the project. For example:

```
cmake -DENABLE_DEBUG=ON -DUSE_EXTRA_LIBRARY=OFF ..
```

This will configure the project to enable debug mode and disable the extra library feature.

## CMake GUI:

In the CMake GUI, users will see a list of options with checkboxes. They can toggle the checkboxes to enable or disable features as needed, and the values will be set when they generate the build files.

---

# Practical Use Case

Let’s say you're developing a project with optional features, and you want to provide users the ability to easily enable or disable certain parts of the project. For example, you might want to optionally include an external library or enable specific build configurations (like debug or release mode).

```
option(BUILD_EXAMPLES "Build example programs" OFF)
option(USE_MYSQL "Enable MySQL support" ON)

if(BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

if(USE_MYSQL)
    find_package(MySQL REQUIRED)
    add_definitions(-DUSE_MYSQL)
    message(STATUS "MySQL support enabled")
else()
    message(STATUS "MySQL support disabled")
endif()

add_executable(MyApp main.cpp)
```

# Key Takeaways:

* `option()` defines a **boolean toggle** that can be used to enable or disable features during the CMake configuration.

* These options make it easy to customize the build process without changing the source code.

* The options can be accessed using CMake’s **GUI** or the **command line** to allow users to control which parts of the project are built and how they are configured.

Let me know if you'd like more examples or further clarification\!

