---
tags:
  - FORMAT
  - C
---
Certainly\! In CMake, the `set()` command is used to **define and assign values to variables**. These variables can be used to store paths, flags, lists, and many other types of data that can be utilized throughout your CMake configuration process.

# Syntax of `set()`

```
set(VAR_NAME VALUE [CACHE CACHE_TYPE DESCRIPTION])
```

*   
  **`VAR_NAME`**: The name of the variable you want to define.

* **`VALUE`**: The value you want to assign to the variable.

* **`CACHE`** (optional): If you want to make the variable configurable via the CMake GUI or command line, you can add it to the CMake **cache**. This means the variable will be stored in the CMake cache, and users can modify it when configuring the project.

* **`CACHE_TYPE`** (optional): Specifies the type of the cache variable, such as `STRING`, `BOOL`, or `PATH`.

* **`DESCRIPTION`** (optional): A description of the variable that will appear in the CMake GUI or when configuring the project.

# Basic Usage of `set()`

## Simple Assignment:

You can simply define a variable and assign it a value:

```
set(MY_VAR "Hello, World!")
message(STATUS "MY_VAR = ${MY_VAR}")
```

Here:

* `set(MY_VAR "Hello, World!")` creates a variable `MY_VAR` and assigns the string value `"Hello, World!"` to it.

* `message(STATUS "MY_VAR = ${MY_VAR}")` prints the value of `MY_VAR` to the console during the CMake configuration process.

## Lists:

CMake variables can store lists (i.e., multiple values separated by semicolons). This is useful when you want to define a collection of items, such as source files or directories.

```
set(SOURCES main.cpp app.cpp utils.cpp)
message(STATUS "SOURCES = ${SOURCES}")
```

Here:

* `set(SOURCES main.cpp app.cpp utils.cpp)` defines a list of source files for the project.

* The `message()` command prints the value of the `SOURCES` list.

To access individual items in the list, you can use the `${SOURCES}` variable or refer to specific indices like `${SOURCES_0}`.

## Cache Variables:

You can define variables that will be stored in the CMake **cache**, making them configurable by the user through the CMake GUI or the command line.

```
set(MY_OPTION "ON" CACHE BOOL "Enable or disable feature X")
```

Here:

* `set(MY_OPTION "ON" CACHE BOOL "Enable or disable feature X")` creates a **cache variable** called `MY_OPTION` with a default value of `"ON"`. This variable will appear in the CMake GUI or command line as a configurable boolean option.

* The user can change this value to `"ON"` or `"OFF"` when configuring the project.

## Paths and Directories:

You can define paths or directories in variables. These are often used to store file locations or dependencies.

```
set(MY_PROJECT_PATH "${CMAKE_SOURCE_DIR}/src")
message(STATUS "Project source directory: ${MY_PROJECT_PATH}")
```

Here:

* `set(MY_PROJECT_PATH "${CMAKE_SOURCE_DIR}/src")` sets the variable `MY_PROJECT_PATH` to a specific path, such as the `src` directory in the project.

## Appending to Variables:

If you need to append values to a variable (like adding source files or include directories), you can use `set()` to append them:

```
set(MY_LIST "Item1" "Item2")
list(APPEND MY_LIST "Item3" "Item4")
message(STATUS "MY_LIST = ${MY_LIST}")
```

Here:

* `list(APPEND MY_LIST "Item3" "Item4")` adds `"Item3"` and `"Item4"` to the `MY_LIST` variable. After this, `MY_LIST` will contain `"Item1;Item2;Item3;Item4"`.

---

# Advanced Usage

## Using `CACHE` for User Configuration:

You can use the `CACHE` option to create configurable variables that users can modify. For example, if you have an option to enable or disable a specific feature:

```
set(USE_FEATURE_X OFF CACHE BOOL "Enable feature X")
```

This creates a cache variable `USE_FEATURE_X` with a default value of `OFF`. The user can configure it via the CMake GUI or command line:

```
cmake -DUSE_FEATURE_X=ON ..
```

## Overriding Cache Variables:

If a variable is already defined in the CMake cache, calling `set()` again without `CACHE` will **overwrite** its value in the current session but will not update the cache permanently. To override the cache value, you can use the `CACHE` keyword again:

```
set(MY_CACHE_VAR "New Value" CACHE STRING "Description of the cache variable")
```

This will update the cached value of `MY_CACHE_VAR` in CMake.

## Combining Lists and String Values:

You can combine lists and string values in variables. For example:

```
set(MY_LIST "Item1" "Item2")
set(MY_LIST "${MY_LIST};Item3;Item4")
message(STATUS "MY_LIST = ${MY_LIST}")
```

Here:

* We add new items to the existing list by appending them as a string to the variable, resulting in `"Item1;Item2;Item3;Item4"`.

---

# Common Use Cases

* **Storing Paths**: Often used for storing paths to libraries, executables, or dependencies.

```
set(LIBRARY_PATH "${CMAKE_SOURCE_DIR}/libs")
```

*   
  **Build Flags**: Store build flags or options that control compilation behavior.

```
set(CMAKE_CXX_FLAGS "-Wall -Wextra")
```

*   
  **Conditional Configurations**: Store configuration values that control which parts of the project are built.

```
set(ENABLE_TESTING ON CACHE BOOL "Enable testing")
```

*   
  **Source Files**: Store lists of source files or resources for building targets.

```
set(SOURCES main.cpp app.cpp utils.cpp)
```

---

# Key Takeaways

* The `set()` command in CMake is used to define and assign values to variables.

* You can store strings, lists, paths, flags, and more using `set()`.

* Using `CACHE` with `set()` makes the variable configurable by the user, such as via the CMake GUI or command line.

* You can append values to a variable (e.g., lists) with `list(APPEND ...)` or by reassigning the variable.

* `set()` is an essential tool for controlling build settings, paths, source files, and user preferences during configuration.

Let me know if you'd like further clarification or more examples\!

Certainly\! In addition to defining variables within CMake, you can also use **environment variables** in CMake to configure your build or customize the behavior of your project. Environment variables are system-wide or user-specific variables that provide information to applications and processes (such as paths, configuration values, or system settings). CMake allows you to access and use environment variables in your build process.

# How to Use Environment Variables in CMake

## Accessing Environment Variables

You can use CMake's `getenv()` function to retrieve the value of an environment variable. It will return the value of the environment variable if it exists or `NOTFOUND` if it doesn't.

```
 Get the value of an environment variable
set(MY_ENV_VAR $ENV{MY_ENV_VAR})
message(STATUS "MY_ENV_VAR = ${MY_ENV_VAR}")
```

In this example:

* `$ENV{MY_ENV_VAR}` accesses the environment variable `MY_ENV_VAR`.

* `set(MY_ENV_VAR ...)` stores the value of the environment variable in the CMake variable `MY_ENV_VAR`.

* `message(STATUS ...)` prints the value to the console during the CMake configuration process.

## Using Environment Variables to Set Paths

Environment variables are often used to specify paths to external libraries, compilers, or tools. For example, if you want to specify a path to a library that may be installed in a custom location, you can use an environment variable like this:

```
 Assume the environment variable MY_LIB_PATH points to the location of the library
set(LIBRARY_PATH $ENV{MY_LIB_PATH})
if(LIBRARY_PATH)
    message(STATUS "Library path: ${LIBRARY_PATH}")
else()
    message(WARNING "MY_LIB_PATH environment variable is not set.")
endif()
```

In this example:

* The value of `MY_LIB_PATH` is assigned to the `LIBRARY_PATH` variable.

* If `MY_LIB_PATH` is not set, a warning message is printed.

## Setting Environment Variables for the Build Process

You can set environment variables for specific build commands using the `set_property()` command with the `GLOBAL` or `TARGET` options. This is useful if you need to modify the environment for specific build steps, such as running custom commands or tests.

```
 Set an environment variable for a custom command
set_property(DIRECTORY APPEND PROPERTY CMAKE_ENV_MY_VAR "MyValue")

 This would set MY_VAR to "MyValue" when running custom commands or tests
```

In this example:

* `set_property()` is used to set the environment variable `MY_VAR` to `"MyValue"` for custom commands or tests in the build.

## Using Environment Variables in Cache Variables

You can also use environment variables to initialize CMake cache variables. This can be helpful for configuring paths or other settings that are system-dependent.

```
 Use an environment variable to initialize a cache variable
set(MY_LIBRARY_PATH $ENV{MY_LIBRARY_PATH} CACHE PATH "Path to My Library")
```

This example:

* Initializes the `MY_LIBRARY_PATH` CMake cache variable with the value of the environment variable `MY_LIBRARY_PATH`.

## Setting Environment Variables in CMake

CMake itself doesn’t directly provide a command to set **system-wide** environment variables. However, you can modify the environment for specific commands or targets. If you need to set environment variables globally for the entire CMake process, you might need to do that in the shell or batch script running CMake.

```
 Example for Unix-based systems
export MY_ENV_VAR="Some Value"
cmake ..
```

For Windows:

```
set MY_ENV_VAR=Some Value
cmake ..
```

This sets the environment variable `MY_ENV_VAR` for the duration of the CMake configuration process, allowing CMake to access it during the configuration.

---

# Key Differences Between CMake Variables and Environment Variables

* **CMake Variables**: These are variables that are set within the `CMakeLists.txt` files using commands like `set()`, and they are used for managing paths, options, and other configuration parameters. These variables are only accessible within the CMake configuration and the build system it generates.

* **Environment Variables**: These are external variables defined at the system level (or per process) and can be accessed by CMake, the operating system, or other tools that are part of the build process. They are used to configure system paths, control external tools, or pass values to the build system that are not specific to CMake.

---

# Practical Example Combining CMake Variables and Environment Variables

Let's say you have an external library whose location is determined by an environment variable, and you want to pass that location to your build system using CMake.

```
 Set the path to an external library using an environment variable
set(EXTERNAL_LIB_PATH $ENV{EXTERNAL_LIB_PATH})

 Check if the environment variable is set
if(EXTERNAL_LIB_PATH)
    message(STATUS "Using external library from ${EXTERNAL_LIB_PATH}")
  # Link the external library
    link_directories(${EXTERNAL_LIB_PATH})
else()
    message(FATAL_ERROR "EXTERNAL_LIB_PATH environment variable not set!")
endif()

 Define your target
add_executable(MyApp main.cpp)

 Link external libraries if the path was set
target_link_libraries(MyApp external_library)
```

In this example:

* The value of the `EXTERNAL_LIB_PATH` environment variable is used to determine where to find the external library.

* If the environment variable is not set, CMake will stop the configuration with a fatal error message.

* If it’s set, the external library is linked to the target `MyApp`.

---

# Summary

* **CMake Variables**: Defined using `set()` within `CMakeLists.txt`, used to configure build options and paths.

* **Environment Variables**: External variables that are available to the system and processes. You can access them in CMake using `$ENV{VAR_NAME}`.

* **`set()`** is used to define CMake variables, while `$ENV{}` is used to access environment variables.

* **Environment Variables in Build Process**: You can set environment variables for specific targets or commands using `set_property()` or modify them before running CMake.

* **Use Cases**: Environment variables are often used to pass system-wide settings (like paths to libraries) into CMake or control build options in a flexible, system-independent way.

Let me know if you need further clarification or examples\!

