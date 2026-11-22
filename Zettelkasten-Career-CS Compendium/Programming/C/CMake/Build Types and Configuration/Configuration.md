---
tags:
  - C
  - FORMAT
---
# Setting Default Build Type with `CMAKE_BUILD_TYPE`

In CMake, the `CMAKE_BUILD_TYPE` variable specifies the **build type** to use when configuring a project. This variable is commonly used to set the type of optimization and debugging information that should be applied during the build process.

By default, CMake does not set a value for `CMAKE_BUILD_TYPE`. This means that unless you explicitly define it, no build type will be applied. In such cases, the generator (like **Make** or **Ninja**) will build in the default configuration, which could vary based on the build tool used (for example, **Debug** or **Release** by default in some cases). To set a default build type, you can specify it in your CMake project configuration.

# Setting a Default Build Type

You can set a default build type in your CMake project by using a conditional check within the `CMakeLists.txt` file. This way, if the user doesn't specify a build type when running CMake, it will automatically fall back to the default you specify.

```
 Check if CMAKE_BUILD_TYPE is not set (empty or undefined)
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the build type: Debug, Release, RelWithDebInfo, MinSizeRel" FORCE)
endif()
```

*   
  **Explanation**:

  * The `if(NOT CMAKE_BUILD_TYPE)` check ensures that CMake sets a default build type if the user has not specified one.

  * The `set()` command sets the default build type to **Release** (you can change this to any other type as needed).

  * The `CACHE` option ensures that this value will be visible and configurable via the CMake GUI or command-line configuration.

  * The `FORCE` argument ensures that even if the user has previously set `CMAKE_BUILD_TYPE`, it will be overridden by this default.

# Specifying the Build Type with `-DCMAKE_BUILD_TYPE`

When configuring your project using CMake from the command line, you can explicitly specify the build type by passing `-DCMAKE_BUILD_TYPE=type`, where `type` can be one of the common build types, such as **Debug**, **Release**, **RelWithDebInfo**, or **MinSizeRel**.

```
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo ..
cmake -DCMAKE_BUILD_TYPE=MinSizeRel ..
```

*   
  **Explanation**:

  * The `-DCMAKE_BUILD_TYPE=Debug` argument tells CMake to configure the project for debugging (i.e., no optimizations, with debugging symbols).

  * The `-DCMAKE_BUILD_TYPE=Release` argument tells CMake to configure the project for release (i.e., optimizations enabled, no debugging symbols).

  * The `-DCMAKE_BUILD_TYPE=RelWithDebInfo` tells CMake to configure the project with optimizations, but also include debugging information.

  * The `-DCMAKE_BUILD_TYPE=MinSizeRel` tells CMake to configure the project with optimizations that minimize the size of the resulting binary.

# Example Use Case: Passing Build Types to Generators

When you invoke CMake with a build tool like **Make** or **Ninja**, you can pass the build type to the generator to control the configuration.

## Example 1: Using Make as the Generator

```
cmake -DCMAKE_BUILD_TYPE=Release ..
make
```

In this example:

* `cmake -DCMAKE_BUILD_TYPE=Release ..` configures the project for a **Release** build.

* `make` then builds the project using the **Release** configuration.

## Example 2: Using Ninja as the Generator

```
cmake -G Ninja -DCMAKE_BUILD_TYPE=Debug ..
ninja
```

In this example:

* `cmake -G Ninja -DCMAKE_BUILD_TYPE=Debug ..` configures the project for a **Debug** build using **Ninja** as the generator.

* `ninja` is used to build the project with the specified **Debug** configuration.

# Using Multi-Config Generators

For **multi-config** generators like **Visual Studio** or **Xcode**, the build type is not set during the initial CMake configuration. Instead, you choose the build type when building the project. In these cases, you do not need to specify `CMAKE_BUILD_TYPE` at configuration time because the generator allows you to select the build type later.

For example, in **Visual Studio** or **Xcode**, you can choose the build type from the IDE or specify it when building from the command line:

## Visual Studio Example:

```
cmake --build . --config Release
```

Here:

* The build type (Release, Debug, etc.) is selected when you run the build command, not at the configuration stage.

## Xcode Example:

```
xcodebuild -configuration Release
```

This way, you can choose the configuration at the time of building, rather than when you initially configure the project with CMake.

# Conditional Customizations Based on Build Type

You can customize certain settings in your `CMakeLists.txt` file based on the selected build type. For example, you might want to add specific compile flags for different build types.

```
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(CMAKE_CXX_FLAGS_DEBUG "-g -O0")
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
    set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
endif()
```

*   
  This example conditionally adds specific flags for **Debug** and **Release** builds. You can do this for other build types as well.

---

# Summary

* **`CMAKE_BUILD_TYPE`** is used to define the build configuration (such as Debug, Release, RelWithDebInfo, or MinSizeRel) in CMake.

* By default, CMake doesn’t set a build type, so you often need to pass `-DCMAKE_BUILD_TYPE` to specify one during configuration.

* You can set a **default build type** in your `CMakeLists.txt` to avoid requiring users to specify it manually.

* For **multi-config generators** (like Visual Studio and Xcode), the build type is chosen during the build process, not at configuration time.

* You can **pass the build type** using `-DCMAKE_BUILD_TYPE=type` to control optimization, debugging, and other settings.

This flexibility allows you to control your build settings based on the needs of the project, whether you’re developing locally or preparing a release version of your software.

