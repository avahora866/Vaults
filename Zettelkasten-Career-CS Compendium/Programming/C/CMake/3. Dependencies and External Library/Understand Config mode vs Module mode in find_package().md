---
tags:
  - FORMAT
  - C
---
# Config Mode vs Module Mode in `find_package()`

In CMake, the `find_package()` command is used to locate and configure external dependencies (such as libraries or tools) that your project relies on. However, there are two primary ways CMake searches for these dependencies: **Config mode** and **Module mode**. Each method has different characteristics, and understanding how they work will help you choose the right one for your project.

# Config Mode (Preferred for Modern CMake)

**Config mode** is the newer and more preferred method for finding packages in CMake. In this mode, the package you're searching for provides its own configuration files, typically named `<PackageName>Config.cmake`. These files are responsible for defining variables and settings needed by CMake to integrate the package into your build.

## Characteristics of Config Mode:

* **Package Provides Configuration Files**: The package itself includes a `Config.cmake` file (e.g., `BoostConfig.cmake`, `Qt5Config.cmake`), which provides all the necessary information CMake needs to integrate the package.

* **Direct Integration**: The `Config.cmake` file typically sets variables such as include paths, library paths, and other settings specific to the package.

* **More Accurate and Reliable**: Since the package is responsible for defining the necessary variables and settings, it ensures that CMake can correctly configure the package and avoid hardcoded paths or assumptions.

* **Preferred for Modern Libraries**: Most modern libraries and packages support this method, making it the go-to choice for finding external packages in modern CMake-based projects.

## How Config Mode Works:

When you call `find_package()` in Config mode, CMake looks for a `<PackageName>Config.cmake` file in its module paths or directories specified by `CMAKE_PREFIX_PATH`.

For example:

```
find_package(Boost REQUIRED CONFIG)
```

*   
  This will search for `BoostConfig.cmake` in directories listed in `CMAKE_PREFIX_PATH` or standard search locations (like `/usr/local/lib/cmake/Boost`).

* The `Config.cmake` file sets up variables such as `Boost_INCLUDE_DIRS` and `Boost_LIBRARIES`, which can then be used in your project.

---

# Module Mode (Older Method)

**Module mode** is the older and more traditional method in CMake for finding packages. In this mode, CMake looks for a module file named `Find<PackageName>.cmake` (e.g., `FindBoost.cmake`, `FindZLIB.cmake`). These module files contain predefined logic that CMake uses to search for the package and configure it.

## Characteristics of Module Mode:

* **CMake Provides Search Logic**: In this mode, CMake itself provides the logic to find the package, rather than the package supplying a configuration file.

* **Less Accurate**: The search logic in the module file might rely on specific assumptions about the package’s installation paths or structure, making it less flexible than Config mode.

* **Potentially More Complex**: The module files might contain additional search logic to handle common installation paths, but they can sometimes require manual tweaks if your package is installed in a non-standard location.

* **Older Packages and Legacy Support**: Many older packages and libraries (especially those that have been around since the early days of CMake) still use module files, and CMake continues to support this method for backward compatibility.

## How Module Mode Works:

When you call `find_package()` in Module mode, CMake looks for a `Find<PackageName>.cmake` module file in its module directories or system paths.

For example:

```
find_package(Boost REQUIRED)
```

*   
  This will search for `FindBoost.cmake` in CMake's module directories (e.g., `/usr/share/cmake/Modules/`).

* If found, `FindBoost.cmake` contains the logic to search for Boost's headers and libraries, and it sets variables such as `Boost_INCLUDE_DIRS` and `Boost_LIBRARIES`.

---

# Key Differences Between Config Mode and Module Mode

| Feature | Config Mode | Module Mode |
| ----- | ----- | ----- |
| **Configuration File** | Uses `<PackageName>Config.cmake` provided by the package itself | Uses `Find<PackageName>.cmake`, a CMake-provided module file |
| **Package Responsibility** | Package is responsible for providing configuration details | CMake provides logic to locate the package |
| **Accuracy** | More accurate, as the package knows its own settings | Less accurate, may assume standard locations |
| **Package Types** | Preferred for modern packages and libraries | Often used for older packages or legacy systems |
| **Flexibility** | More flexible, handles custom paths better | Less flexible, might need manual tweaking for non-standard installations |
| **Typical Use Case** | Modern, self-configuring packages like Boost, Qt, OpenSSL, etc. | Older or legacy packages, packages without CMake support |
| **CMake Version** | Introduced in CMake 3.x, more common in recent versions | Older method, still supported but less common |

---

# Example: Config Mode for Finding Boost

Here’s an example of how you would use Config mode to find Boost:

```
find_package(Boost 1.70 REQUIRED CONFIG)
```

*   
  CMake will look for the `BoostConfig.cmake` file in the system directories or the directories specified by `CMAKE_PREFIX_PATH`.

* If Boost is found, the necessary variables (`Boost_INCLUDE_DIRS`, `Boost_LIBRARIES`, etc.) will be set automatically.

* If Boost is not found, CMake will print an error because of the `REQUIRED` keyword.

## Handling Non-Standard Locations:

If Boost is installed in a non-standard directory, you can specify additional paths using `CMAKE_PREFIX_PATH` or the `HINTS` option:

```
find_package(Boost REQUIRED CONFIG HINTS /path/to/boost)
```

---

# Example: Module Mode for Finding Boost

Here’s an example of how you would use Module mode to find Boost:

```
find_package(Boost REQUIRED)
```

*   
  CMake will look for the `FindBoost.cmake` module file in its module directories or locations defined by `CMAKE_MODULE_PATH`.

* If Boost is found, CMake will set the necessary variables (e.g., `Boost_INCLUDE_DIRS`, `Boost_LIBRARIES`).

* The module will use predefined logic to locate Boost, which may rely on specific assumptions about where Boost is installed.

## Handling Non-Standard Locations:

If Boost is installed in a custom location, you can specify the search path using `CMAKE_MODULE_PATH`:

```
set(CMAKE_MODULE_PATH /path/to/cmake/modules ${CMAKE_MODULE_PATH})
find_package(Boost REQUIRED)
```

---

# Summary

* **Config Mode** is the preferred and modern way of finding packages in CMake. It relies on the package providing its own `Config.cmake` file, which is more accurate and flexible.

* **Module Mode** is the older method, where CMake uses a `Find<PackageName>.cmake` module file to locate the package. While still supported, it is less accurate and might require manual adjustments for non-standard installations.

* **When to Use Config Mode**: For modern libraries that support CMake configuration files, or when you want a more robust and flexible integration.

* **When to Use Module Mode**: For older packages that do not support Config mode or for legacy systems where module files are still in use.

