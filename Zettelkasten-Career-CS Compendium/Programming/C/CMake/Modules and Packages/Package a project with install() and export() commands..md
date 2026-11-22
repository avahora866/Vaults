---
tags:
  - FORMAT
  - C
---
# Packaging a CMake Project with `install()` and `export()`

CMake provides tools to easily package and install projects, enabling developers to distribute their software in a way that others can easily consume and integrate. The two most commonly used commands for packaging are `install()` and `export()`. These commands allow you to specify which files to install, where to install them, and to export project details to make it easier for other projects to find and use your installed software.

Let’s break down how to use `install()` and `export()` commands in a CMake project.

---

# Using the `install()` Command

The `install()` command is used to specify which files should be installed and where they should be placed on the target system. It is a key part of making your project distributable. This command can be used to install libraries, executables, headers, configuration files, and more.

## Syntax:

```
install(TARGETS <target1> <target2> ...
  RUNTIME DESTINATION <path>   # For executables
  LIBRARY DESTINATION <path>   # For shared libraries
  ARCHIVE DESTINATION <path>   # For static libraries
  INCLUDES DESTINATION <path>  # For headers
)

install(DIRECTORY <source_dir> DESTINATION <target_dir> ...)

install(FILES <file1> <file2> DESTINATION <path>)
```

## Example of Installing Targets:

```
 Install an executable
install(TARGETS MyApp
    RUNTIME DESTINATION bin
)

 Install a library
install(TARGETS MyLibrary
    LIBRARY DESTINATION lib
)

 Install header files
install(DIRECTORY include/ DESTINATION include)
```

In this example:

* `install(TARGETS MyApp ...)` installs the `MyApp` executable into the `bin` directory.

* `install(TARGETS MyLibrary ...)` installs the `MyLibrary` shared library into the `lib` directory.

* `install(DIRECTORY ...)` installs the contents of the `include/` directory to `include/` on the target system.

---

# Using the `export()` Command

The `export()` command is used to create a CMake "package" or "config" file that describes the structure of your project, its dependencies, and how to use it in other CMake projects. This is typically used to export the build settings (like the installed targets) to be reused by other projects, making it easier to integrate your library or software into another CMake-based project.

## Syntax:

```
export(TARGETS <target1> <target2> ...
    FILE <path_to_export_file>
)
```

## Example of Exporting a Project Configuration:

```
 Export the targets for use by other projects
install(TARGETS MyApp MyLibrary
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
)

 Export the configuration to a file
export(TARGETS MyApp MyLibrary
    FILE "${CMAKE_BINARY_DIR}/MyLibraryConfig.cmake"
)
```

Explanation:

* The `install(TARGETS ...)` command installs both the executable and the library to the target system.

* The `export()` command creates a `MyLibraryConfig.cmake` file that describes how to link and use the `MyApp` and `MyLibrary` targets.

## Why Use `export()`?

By exporting the targets in your project, you allow other projects to easily find and link against your targets by simply including your exported CMake configuration file. This eliminates the need for other projects to manually set up paths to libraries or headers.

---

# Packaging Your Project for Distribution

To package your project so it can be easily distributed (e.g., as a `.tar.gz` or `.zip` file), you typically use the `CMAKE_INSTALL_PREFIX` variable to specify the installation directory. You can then run the `install()` commands to install your project to this directory.

## Example: Defining Installation Prefix

```
 Set the install prefix to the desired location
set(CMAKE_INSTALL_PREFIX "/path/to/install")

 Define the install rules
install(TARGETS MyApp
    RUNTIME DESTINATION bin
)
```

To package your project, you would typically run the following commands in the build directory:

```
cmake -DCMAKE_INSTALL_PREFIX=/path/to/install ..
make
make install
```

After running `make install`, all files specified in the `install()` commands are copied to the appropriate directories under `/path/to/install`.

---

# Exporting a CMake Package for Consumption by Other Projects

Once you have installed your targets, you may want to create a CMake package configuration file that other projects can use to find and link against your software. You can achieve this using the `find_package()` mechanism in CMake, combined with the configuration files exported by your project.

## Example: Creating a CMake Package

Here’s how you can export a CMake configuration and use it in other projects:

1. **Create a CMake Configuration File:**

```
 Install targets and export package configuration
install(TARGETS MyLibrary
    EXPORT MyLibraryConfig
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    INCLUDES DESTINATION include
)

 Export the CMake configuration file
export(EXPORT MyLibraryConfig
    FILE "${CMAKE_BINARY_DIR}/MyLibraryConfig.cmake"
)
```

2.   
   **Package Configuration:**

```
 Create the package configuration file
install(EXPORT MyLibraryConfig
    DESTINATION lib/cmake/MyLibrary
)
```

3.   
   **Using Your Package in Another Project:**

In another project, you can use the `find_package()` command to discover your library:

```
 Specify the path where the package is installed
list(APPEND CMAKE_PREFIX_PATH "/path/to/installed/lib/cmake/MyLibrary")

 Find the package
find_package(MyLibrary REQUIRED)

 Link your target with the found library
add_executable(MyProject main.cpp)
target_link_libraries(MyProject PRIVATE MyLibrary)
```

In this example:

* `install(EXPORT ...)` exports the project’s configuration, enabling other projects to use `find_package()` to find the `MyLibrary` targets.

* `find_package(MyLibrary REQUIRED)` tells the new project to search for `MyLibrary` in the specified paths, making it easy to link against the library.

---

# Using `CMakePackageConfigHelpers` to Generate Package Config Files

CMake has a helper module called `CMakePackageConfigHelpers` that simplifies the process of generating package configuration files (`*.cmake` files) when exporting your project.

For example, you can use `configure_package_config_file()` to create a package config file and then install it with `install()`:

```
include(CMakePackageConfigHelpers)

 Configure the package config file template
configure_package_config_file(
    MyLibraryConfig.cmake.in
    "${CMAKE_BINARY_DIR}/MyLibraryConfig.cmake"
    INSTALL_DESTINATION lib/cmake/MyLibrary
)

 Install the config file
install(FILES "${CMAKE_BINARY_DIR}/MyLibraryConfig.cmake"
    DESTINATION lib/cmake/MyLibrary
)
```

Here:

* `configure_package_config_file()` takes an input file (`MyLibraryConfig.cmake.in`) and generates a config file at the specified output location.

* The generated config file is installed with `install(FILES ...)` to the appropriate directory.

---

# Example Workflow for Packaging and Installing a Project

To summarize how to package and install a project, here’s a complete example workflow:

```
 CMakeLists.txt for MyLibrary

cmake_minimum_required(VERSION 3.10)
project(MyLibrary)

add_library(MyLibrary STATIC src/mylib.cpp)

 Install the library and headers
install(TARGETS MyLibrary
    EXPORT MyLibraryConfig
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    INCLUDES DESTINATION include
)

 Export the package configuration
export(EXPORT MyLibraryConfig
    FILE "${CMAKE_BINARY_DIR}/MyLibraryConfig.cmake"
)

 Install the configuration file
install(FILES "${CMAKE_BINARY_DIR}/MyLibraryConfig.cmake"
    DESTINATION lib/cmake/MyLibrary
)

 Example of installing headers
install(DIRECTORY include/ DESTINATION include)
```

*   
  This CMakeLists.txt defines and installs a static library, exports its configuration, and makes it easy for other projects to use it via `find_package()`.

---

# Summary

* **`install()`** is used to copy built targets (executables, libraries, headers) to appropriate directories for system installation.

* **`export()`** is used to generate configuration files that describe the build targets, making it easier for other projects to find and link against your targets.

* Packaging involves setting a proper installation prefix, installing files, and using `export()` to make your targets available for other projects.

* Use `CMakePackageConfigHelpers` to simplify the generation of configuration files for CMake packages.

