---
tags:
  - FORMAT
  - C
---
# Using `ExternalProject_Add()` for Larger or More Complex Projects in CMake

CMake’s `ExternalProject` module allows you to download, build, and install external projects or dependencies as part of the CMake build process. This is particularly useful for larger or more complex projects where you need to integrate external software that might involve more complicated build steps (e.g., configuring with different flags, building in a separate directory, or handling third-party software with specific build systems like `autotools` or `make`).

`ExternalProject_Add()` is more flexible than `FetchContent` because it allows you to fully control the download, build, and installation process. This command is typically used for handling projects that require special configuration steps or need to be built in isolation from your main project.

---

# Key Features of `ExternalProject_Add()`

1. **Separate Build Process**: `ExternalProject_Add()` creates a completely isolated build process for the external project, meaning it does not interfere with your main project’s CMake build process.

2. **Custom Commands**: You can specify custom commands for downloading, building, installing, and testing the external project.

3. **Download, Build, and Install**: It allows you to fetch and build external projects or libraries directly from source repositories or archives and install them into specified directories.

4. **External Dependencies**: You can download and manage large or complex dependencies that may have their own build systems or configuration options.

# Key Commands and Workflow

1. **`ExternalProject_Add()`**: The main function used to define an external project and specify how it will be handled during the build process.

2. **Custom Steps**: You can specify custom commands for downloading, configuring, building, installing, and testing the external project using options like `DOWNLOAD_COMMAND`, `BUILD_COMMAND`, `INSTALL_COMMAND`, and others.

---

# Basic Usage of `ExternalProject_Add()`

Here’s a simple example to demonstrate how `ExternalProject_Add()` can be used to download, build, and install an external project.

## Example `CMakeLists.txt` for Using `ExternalProject_Add()`

```
cmake_minimum_required(VERSION 3.14)
project(MyProject)

 Include the ExternalProject module
include(ExternalProject)

 Add an external project (e.g., wget)
ExternalProject_Add(
    MyLibrary
    GIT_REPOSITORY https://github.com/someuser/somelibrary.git
    GIT_TAG        master
    PREFIX         ${CMAKE_BINARY_DIR}/_deps
    CMAKE_ARGS     -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
    BUILD_COMMAND  make
    INSTALL_COMMAND make install
)

 Add the external project's include and library directories after it is installed
add_dependencies(MyApp MyLibrary)
```

# Explanation of the Example

1. **`ExternalProject_Add()`**:

   * **`MyLibrary`**: The name of the external project (can be any name).

   * **`GIT_REPOSITORY`**: Specifies the URL of the Git repository for the external project.

   * **`GIT_TAG`**: Specifies the Git commit, branch, or tag to checkout. In this case, we are using the `master` branch.

   * **`PREFIX`**: Specifies the directory where the external project will be downloaded and built. This creates a subdirectory under the build directory (`${CMAKE_BINARY_DIR}/_deps` in this case) to store the downloaded files.

   * **`CMAKE_ARGS`**: Passes arguments to the external project’s CMake configuration, such as the installation directory (`<INSTALL_DIR>` is a special variable that points to the installation directory for the external project).

   * **`BUILD_COMMAND`**: The command used to build the external project. Here we use `make`, but it could be any build tool appropriate for the external project.

   * **`INSTALL_COMMAND`**: The command used to install the external project after it has been built. In this case, `make install`.

2. **`add_dependencies()`**:

   * This ensures that your main project (`MyApp`) will depend on `MyLibrary` being built first. This means that CMake will make sure `MyLibrary` is downloaded, built, and installed before starting to build your application.

---

# Advanced Example with Custom Commands

In some cases, you might need to use a non-CMake build system for the external project (e.g., `make` or `autotools`). `ExternalProject_Add()` allows you to define custom commands for these cases.

Here’s an example where the external project uses `autotools` for configuration and building:

## Example `CMakeLists.txt` for an External Project with `autotools`

```
cmake_minimum_required(VERSION 3.14)
project(MyProject)

 Include the ExternalProject module
include(ExternalProject)

 Add an external project that uses autotools for configuration and build
ExternalProject_Add(
    MyAutotoolsProject
    GIT_REPOSITORY https://github.com/someuser/myautotoolsproject.git
    GIT_TAG        master
    PREFIX         ${CMAKE_BINARY_DIR}/_deps
    CONFIGURE_COMMAND ./configure --prefix=<INSTALL_DIR>
    BUILD_COMMAND   make
    INSTALL_COMMAND make install
    TEST_COMMAND    make check
)

 Add a dependency to ensure the external project is built first
add_dependencies(MyApp MyAutotoolsProject)
```

# Explanation of the Advanced Example

1. **`CONFIGURE_COMMAND`**: Specifies the command used to configure the external project. In this case, `./configure --prefix=<INSTALL_DIR>` is used for an autotools-based project.

2. **`BUILD_COMMAND`**: Uses `make` to build the project after it’s been configured.

3. **`INSTALL_COMMAND`**: The command to install the external project after it’s been built (`make install`).

4. **`TEST_COMMAND`**: Optionally, you can specify a command to run tests on the external project after it’s built (`make check` in this case).

5. **Dependencies**: The `add_dependencies()` function ensures that `MyApp` depends on the successful build of `MyAutotoolsProject`.

---

# Why Use `ExternalProject_Add()` for Larger or Complex Projects?

`ExternalProject_Add()` is particularly useful for larger or more complex projects where the external dependency might require:

* **A non-CMake build system**: Many older projects still use `autotools`, `make`, or other systems, and `ExternalProject_Add()` can accommodate those by letting you run the appropriate commands.

* **Complex configuration**: You might need to run multiple configuration steps or use specific flags during the build process. `ExternalProject_Add()` gives you fine-grained control over how the external project is configured and built.

* **Isolated builds**: It creates isolated build environments for external projects, so they won’t interfere with your main project’s CMake configuration.

* **Dependencies with different build systems**: If the external project has complex or specific requirements that don’t fit within the scope of a simple CMake integration, `ExternalProject_Add()` provides the flexibility to execute custom commands as needed.

---

# Common Use Cases for `ExternalProject_Add()`

1. **Downloading and building non-CMake projects** (e.g., `autotools`, `make`, or other custom build systems).

2. **Handling large or complex dependencies** that may need to be fetched from external sources and built with special configuration flags.

3. **Managing projects that need to be downloaded, built, and installed before your main project can be built**.

4. **Complex workflows** that involve more than just downloading code, such as additional configuration, testing, or installation steps.

---

# Summary

* **`ExternalProject_Add()`** is a powerful CMake command for integrating external dependencies into your project, especially when these dependencies require complex build steps, non-CMake build systems, or special configurations.

* It allows for complete control over the downloading, building, and installation process, making it ideal for large or complex external projects.

* Use this method when you need more flexibility than `FetchContent` offers, especially for dependencies that don’t use CMake or have their own specific build systems and configuration steps.

* It creates isolated builds for the external projects, ensuring they don’t interfere with your main project’s configuration or build process.

