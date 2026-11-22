---
tags:
  - FORMAT
  - C
---
# Using `find_package()` to Locate External Packages in CMake

In CMake, the `find_package()` command is used to locate external libraries or packages on the system that your project depends on. These external dependencies can be libraries, frameworks, or other tools that your project needs to build or link against. The `find_package()` command searches for the package, verifies its presence, and makes it available for use in your CMake project.

This is an essential feature when you're working with third-party libraries, system libraries, or even custom-built packages, as it allows CMake to automatically detect and configure the necessary settings.

---

# Syntax of `find_package()`

```
find_package(<package-name> [version] [EXACT] [REQUIRED] [CONFIG] [QUIET] [NO_MODULE])
```

*   
  `<package-name>`: The name of the package you're trying to find (e.g., `Boost`, `OpenSSL`, `ZLIB`).

* `[version]`: Optional version specification. You can specify a minimum version or an exact version of the package.

* `[EXACT]`: Ensures the exact version of the package is found. Without it, the version must be greater than or equal to the specified version.

* `[REQUIRED]`: Makes the package mandatory. If not found, CMake will terminate with an error.

* `[CONFIG]`: Specifies that CMake should search for the package in its configuration file (typically named `<package-name>Config.cmake`).

* `[QUIET]`: Suppresses warnings if the package is not found.

* `[NO_MODULE]`: Instructs CMake to skip searching for the package in modules (it only looks for the package's `Config.cmake`).

---

# Common Use Cases for `find_package()`

1. **Locating Installed Libraries**: You can use `find_package()` to locate system libraries that have been installed in standard locations (e.g., Boost, OpenSSL).

2. **Custom External Libraries**: If you have external libraries that are not part of the standard system locations, `find_package()` can help you locate them by specifying paths or using custom `Config.cmake` files.

3. **Enabling or Disabling Features**: Using the `REQUIRED` or `QUIET` options, you can control whether CMake should fail or simply give a warning if a package isn't found.

4. **Configuring External Dependencies**: When using `find_package()`, it provides you with the necessary variables (e.g., paths to headers, libraries) so you can easily link and include the package in your project.

---

# Example 1: Locating a Package Without Versioning

If you're trying to locate a package like `Boost` but don't require a specific version, you can use `find_package()` like this:

```
find_package(Boost REQUIRED)
```

*   
  This will search for `Boost` on your system.

* The `REQUIRED` option ensures that CMake will fail with an error if it cannot find the package.

After locating the package, you can use the variables that the package provides, such as `Boost_INCLUDE_DIRS` and `Boost_LIBRARIES`, to add them to your project:

```
if (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
    target_link_libraries(MyApp ${Boost_LIBRARIES})
endif()
```

---

# Example 2: Specifying a Minimum Version

You can also specify a minimum version for a package. This ensures that the found package meets the version requirement:

```
find_package(Boost 1.70 REQUIRED)
```

*   
  This ensures that CMake will only proceed if it finds **at least version 1.70** of `Boost`.

* If the version is too old, CMake will print an error and stop.

---

# Example 3: Using the `CONFIG` Mode for Modern CMake Packages

Many modern CMake packages provide a `Config.cmake` file that describes the package. In such cases, you can use the `CONFIG` option to search for this configuration file:

```
find_package(OpenSSL CONFIG REQUIRED)
```

*   
  The `CONFIG` flag tells CMake to look for the `OpenSSLConfig.cmake` file in directories where it might be installed.

* The `REQUIRED` flag ensures that the package is mandatory.

This is the preferred approach for CMake packages that include their own `Config.cmake` files (often in modern libraries like `OpenSSL`, `Qt`, etc.).

---

# Example 4: Searching in Specific Directories

If the package is not located in standard system directories, you can tell CMake where to look for the package using `CMAKE_PREFIX_PATH` or by directly specifying the path using the `HINTS` option:

```
find_package(MyLibrary REQUIRED HINTS /path/to/my/library)
```

*   
  This tells CMake to look for `MyLibrary` in the specified `/path/to/my/library` directory.

Alternatively, you can set `CMAKE_PREFIX_PATH` to add a search path globally:

```
cmake -DCMAKE_PREFIX_PATH=/path/to/my/library ..
```

---

# Example 5: Using `find_package()` with Module Mode

Older CMake packages or libraries might not use `Config.cmake` files and instead rely on CMake modules to find the package. In such cases, CMake looks for a file named `Find<PackageName>.cmake` in its module directories.

To search using modules, just use `find_package()` without the `CONFIG` flag:

```
find_package(ZLIB REQUIRED)
```

CMake will automatically search for `FindZLIB.cmake` in its module paths and use that to locate the package.

---

# Handling Optional Packages with `find_package()`

If you have optional dependencies that you want to check for but don’t want the build to fail if they are missing, you can omit the `REQUIRED` flag:

```
find_package(MyLibrary QUIET)
```

*   
  The `QUIET` option suppresses warnings if the package is not found.

* You can then check if the package was found by testing the result of `find_package()` using the `FOUND` variable:

```
if (MyLibrary_FOUND)
    message(STATUS "MyLibrary found!")
else()
    message(STATUS "MyLibrary not found!")
endif()
```

This is useful for optional dependencies, where your project can still build even if a library is not available.

---

# Example 6: Using `find_package()` with `NO_MODULE`

You can also tell CMake to skip the module search and only search for a package's configuration files (`Config.cmake`), even if the package has a module available:

```
find_package(MyLibrary NO_MODULE REQUIRED)
```

*   
  This tells CMake to skip the module search and directly look for the `MyLibraryConfig.cmake` file.

---

# Summary

* **`find_package()`** is used to locate external packages, libraries, and dependencies that your project relies on.

* It allows CMake to automatically detect and configure the necessary settings for external libraries, making them available for inclusion and linking.

* **Options**:

  * `REQUIRED`: Ensures that the package must be found for the build to continue.

  * `CONFIG`: Instructs CMake to look for the package's `Config.cmake` file.

  * `QUIET`: Suppresses warnings when the package is not found.

  * `NO_MODULE`: Skips searching for the package in CMake's modules directory.

* **Version specification** can be used to ensure that a minimum or exact version of the package is found.

* You can specify additional directories to search for packages using `HINTS` or `CMAKE_PREFIX_PATH`.

This powerful command simplifies the process of managing external dependencies in CMake and ensures that your project is configured correctly with the libraries it needs.

