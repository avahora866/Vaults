---
tags:
  - FORMAT
  - C
---
# Writing Custom CMake Modules

In CMake, custom modules allow you to extend the build system by creating your own functionality for use across multiple projects. These modules can be used to define functions, macros, or even find and configure external dependencies that aren't already supported by the built-in CMake modules.

Writing a custom module can help simplify your build process, automate complex configurations, and share reusable functionality.

---

# Basics of Custom CMake Modules

A **CMake module** is simply a `.cmake` file that contains functions, macros, or variable definitions. These modules are typically stored in a directory such as `cmake/Modules/` or any other directory you choose, and they can be included into your `CMakeLists.txt` file using the `include()` command.

## Custom Modules Structure:

```
akeLists.txt           # Main CMake file
cmake/
 └── Modules/
   └── FindMyLibrary.cmake  # Custom module
```

# Step-by-Step Guide to Writing Custom CMake Modules

## Create a Custom Module File

Create a `.cmake` file that defines the functionality you want. For example, let's say we want to create a custom module to find a library called "MyLibrary".

In your project, create a folder `cmake/Modules/` if it doesn’t exist, and then create a file `FindMyLibrary.cmake` inside that folder.

## Define a Find Function in Your Module

A typical use of custom modules is to "find" external dependencies or packages. Here's an example of how you might write a `FindMyLibrary.cmake` module.

### Example: `FindMyLibrary.cmake`

```
 FindMyLibrary.cmake - Custom module to locate MyLibrary

 Check if the MYLIBRARY_PATH variable is already set
if(MYLIBRARY_PATH)
    set(MYLIBRARY_FOUND TRUE)
endif()

 If the library is not found, try searching for it
if(NOT MYLIBRARY_FOUND)
    find_path(MYLIBRARY_INCLUDE_DIR NAMES mylibrary.h PATH_SUFFIXES mylib)
    find_library(MYLIBRARY_LIBRARIES NAMES mylibrary)

    if(MYLIBRARY_INCLUDE_DIR AND MYLIBRARY_LIBRARIES)
        set(MYLIBRARY_FOUND TRUE)
    endif()
endif()

 Set the result of the search as a CMake variable
if(MYLIBRARY_FOUND)
    set(MYLIBRARY_INCLUDE_DIRS ${MYLIBRARY_INCLUDE_DIR})
    set(MYLIBRARY_LIBRARIES ${MYLIBRARY_LIBRARIES})
    message(STATUS "Found MyLibrary: ${MYLIBRARY_LIBRARIES}")
else()
    message(FATAL_ERROR "MyLibrary not found!")
endif()
```

Explanation:

* The `FindMyLibrary.cmake` module tries to locate `mylibrary.h` (the header) and `mylibrary` (the library file) in the system paths.

* It uses `find_path()` to search for the header file and `find_library()` to search for the library file.

* If both the header and library are found, the module sets the `MYLIBRARY_INCLUDE_DIR` and `MYLIBRARY_LIBRARIES` variables.

* If the library is not found, it will produce a fatal error using `message(FATAL_ERROR ...)`.

## Use the Custom Module in `CMakeLists.txt`

After defining the custom module, you can use it in your `CMakeLists.txt` by including it and checking the results.

### Example: Using `FindMyLibrary.cmake` in `CMakeLists.txt`

```
 Main CMakeLists.txt

 Add the path to the cmake/Modules/ folder
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/Modules" ${CMAKE_MODULE_PATH})

 Include the custom FindMyLibrary module
include(FindMyLibrary)

 Use the found library if available
if(MYLIBRARY_FOUND)
    add_executable(MyApp main.cpp)
    target_include_directories(MyApp PRIVATE ${MYLIBRARY_INCLUDE_DIRS})
    target_link_libraries(MyApp PRIVATE ${MYLIBRARY_LIBRARIES})
else()
    message(FATAL_ERROR "MyLibrary was not found, aborting build!")
endif()
```

Explanation:

* `set(CMAKE_MODULE_PATH ...)` adds the directory containing custom modules to CMake's search path for modules.

* `include(FindMyLibrary)` loads the `FindMyLibrary.cmake` module, which attempts to find the library.

* If the library is found, it adds it to the target using `target_include_directories()` and `target_link_libraries()`.

* If the library isn't found, it aborts the build with `message(FATAL_ERROR ...)`.

---

# Other Types of Custom Modules

While `FindXXX.cmake` modules are the most common, custom modules in CMake can also be used to define other kinds of functionality, such as:

1. **Helper Functions and Macros**: Custom modules can contain reusable functions or macros to simplify your CMake scripts.

    **Example: `MyFunctions.cmake`**

```
 MyFunctions.cmake - Define a reusable function for compiling with specific flags
function(add_compile_flags target)
    target_compile_options(${target} PRIVATE -Wall -Wextra)
endfunction()
```

2.   
   You can then include and use this function in your `CMakeLists.txt`:

```
include(MyFunctions)

add_executable(MyApp main.cpp)
d_compile_flags(MyApp)  # Add specific flags to MyApp
```

3.   
   **Creating Targets and Setting Properties**: You can define more complex logic, such as automatically creating targets, adding libraries, or setting properties.

    **Example: `MyLibraryModule.cmake`**

```
 MyLibraryModule.cmake - Create a library target with specific properties
function(create_my_library target_name)
    add_library(${target_name} STATIC mylib.cpp)
    target_include_directories(${target_name} PRIVATE ${CMAKE_SOURCE_DIR}/include)
    target_compile_options(${target_name} PRIVATE -O2)
endfunction()
```

4.   
   Use it in your `CMakeLists.txt`:

```
include(MyLibraryModule)

eate_my_library(MyLibrary)  # Creates MyLibrary target with specific properties
```

---

# Best Practices for Writing Custom Modules

1. **Name your module files clearly**: Custom module filenames should be descriptive of what they do. For example, `FindMyLibrary.cmake`, `MyFunctions.cmake`, etc.

2. **Modularize your code**: If your module grows large, consider breaking it into smaller, focused pieces.

3. **Avoid global variables**: If your module sets variables, make sure they're scoped properly (e.g., use `set()` with the `CACHE` option when needed or scope them within functions).

4. **Check for existing modules**: If you’re writing a `FindXXX.cmake` module, first check if a similar module exists in CMake’s built-in modules or in other third-party module repositories (e.g., the CMake GitHub repository or package management systems like Conan).

5. **Error Handling**: Ensure that your custom module provides clear error messages when something goes wrong. Using `message(FATAL_ERROR ...)` or `message(WARNING ...)` will help other developers troubleshoot issues.

---

# Using Your Custom Module in Other Projects

To reuse your custom CMake module in other projects, you can:

1. **Distribute the module**: Place the module in a `cmake/Modules/` directory in your project or package it for reuse.

2. **Package your CMake module**: If you're distributing a library or software, consider packaging your module so that others can easily integrate it into their own projects.

3. **Use version control**: Store your custom modules in a version-controlled repository (e.g., Git) and reference them in your projects as needed.

---

# Summary

* **Custom CMake modules** extend CMake by defining functions, macros, and search logic to simplify your build process.

* You can write custom modules for various purposes: finding libraries, defining helper functions, or managing build logic.

* The custom module is included via the `include()` command in your `CMakeLists.txt`.

* Best practices include clear naming conventions, modularization, avoiding global variables, and providing good error handling.

* Custom CMake modules can be reused in multiple projects by distributing them through version control or packaging them for use in other projects.

