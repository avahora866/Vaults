---
tags:
  - FORMAT
  - C
---
# Using `FetchContent` to Download and Build Dependencies in CMake

CMake’s `FetchContent` module provides a convenient way to download and build external dependencies as part of your project’s build process. It is particularly useful when you want to include third-party libraries directly in your project, without relying on the user to manually install them. This is often referred to as **"in-source"** or **"out-of-source"** fetching.

The `FetchContent` module allows you to pull external libraries directly from a repository (e.g., GitHub, GitLab) or from a remote archive, and then build them as part of your project. This ensures that the dependencies are automatically downloaded, built, and linked with your project during the CMake configuration and build process.

---

# How `FetchContent` Works

The `FetchContent` module allows you to specify dependencies in your `CMakeLists.txt` file, which are then automatically downloaded, extracted, and built during the CMake configuration step. This can be useful in scenarios where you want to avoid manually downloading or installing dependencies, or where you want to ensure consistency across different environments.

Here’s the general process:

1. **Specify the dependency**: Use the `FetchContent_Declare` command to specify where to fetch the dependency from (e.g., a Git repository or a URL).

2. **Download the dependency**: Use the `FetchContent_MakeAvailable` command to download and make the dependency available to your project.

3. **Use the dependency**: Once downloaded and built, the dependency can be linked to your project just like any other CMake target.

---

# Key Commands in `FetchContent`

1. **`FetchContent_Declare()`**: This is used to declare the external dependency and specify how to fetch it (e.g., from a Git repository, URL, or file).

2. **`FetchContent_MakeAvailable()`**: After declaring the dependency with `FetchContent_Declare()`, this command downloads the content and makes it available for use in your CMake project.

---

# Basic Example of Using `FetchContent`

Here’s a simple example to demonstrate how `FetchContent` works by downloading and building the `fmt` library (a popular C++ formatting library) as part of your project.

## Example `CMakeLists.txt`

```
cmake_minimum_required(VERSION 3.14)
project(MyProject)

 Enable FetchContent module
include(FetchContent)

 Declare the fmt dependency (from GitHub repository)
FetchContent_Declare(
    fmt
    GIT_REPOSITORY https://github.com/fmtlib/fmt.git
    GIT_TAG        master
)

 Fetch and make fmt available
FetchContent_MakeAvailable(fmt)

 Create your project executable
add_executable(MyApp main.cpp)

 Link the fmt library to your executable
target_link_libraries(MyApp PRIVATE fmt::fmt)
```

# Explanation of the Example

1. **`FetchContent_Declare()`**:

   * **`fmt`**: The name of the content to be fetched (this can be any name, but it’s typically the name of the library or dependency).

   * **`GIT_REPOSITORY`**: The Git URL of the repository where the library can be fetched from.

   * **`GIT_TAG`**: This specifies the Git branch, commit, or tag to fetch. In this case, we are using the `master` branch.

2. **`FetchContent_MakeAvailable()`**:

   * This command tells CMake to fetch and make the declared dependency available. In this case, it downloads and builds the `fmt` library.

3. **Linking the Library**:

   * After the `fmt` library is downloaded and built, you can link it to your project using `target_link_libraries()`. The `fmt::fmt` target is automatically created by the `FetchContent` module, so you can link to it like any other CMake target.

---

# Advanced Example: Using a Version Tag and a Specific Commit

If you want to use a specific version of a dependency, you can specify a Git tag, commit hash, or branch. Here’s an example using a specific commit hash for greater control over which version is used.

## Example `CMakeLists.txt` with Commit Hash:

```
cmake_minimum_required(VERSION 3.14)
project(MyProject)

include(FetchContent)

 Declare the fmt dependency (from a specific commit hash)
FetchContent_Declare(
    fmt
    GIT_REPOSITORY https://github.com/fmtlib/fmt.git
  GIT_TAG        4a7b6c4d03c349c6180e8c5d4f8a38599b76b1a1  # Specific commit hash
)

 Fetch and make fmt available
FetchContent_MakeAvailable(fmt)

 Create executable and link the fmt library
add_executable(MyApp main.cpp)
target_link_libraries(MyApp PRIVATE fmt::fmt)
```

*   
  **`GIT_TAG`**: Specifies a specific commit hash (`4a7b6c4d03c349c6180e8c5d4f8a38599b76b1a1`) for more control over the version.

---

# Fetching from a Tarball or Zip Archive

In addition to fetching from Git repositories, `FetchContent` also supports downloading from other sources, such as a tarball or zip archive. Here's an example:

```
cmake_minimum_required(VERSION 3.14)
project(MyProject)

include(FetchContent)

 Declare the dependency from a tarball
FetchContent_Declare(
    SomeLibrary
    URL https://example.com/somelibrary.tar.gz
)

 Fetch and make the library available
FetchContent_MakeAvailable(SomeLibrary)

 Create executable and link the library
add_executable(MyApp main.cpp)
target_link_libraries(MyApp PRIVATE SomeLibrary)
```

*   
  **`URL`**: Specifies the location of the tarball or zip file. CMake will automatically download and extract it.

---

# Benefits of Using `FetchContent`

* **Automatic Dependency Management**: Automatically downloads and integrates dependencies into your project.

* **No Manual Setup**: You don’t need to manually download and set up dependencies or configure package paths. Everything is done automatically during the build process.

* **Consistency**: Ensures that all developers and build systems use the same version of dependencies, which can help avoid compatibility issues.

* **Builds In-Source or Out-of-Source**: `FetchContent` works well both when building inside the source directory or outside the source tree, offering flexibility.

---

# Things to Keep in Mind

* **Source and Build Locations**: By default, `FetchContent` places downloaded content in the build directory. This can be customized if needed using additional options in the `FetchContent_Declare` command.

* **External Dependencies**: While `FetchContent` is a convenient method for including dependencies, it can slow down the configuration process (especially for large repositories) since the dependencies need to be fetched each time CMake is run, unless CMake caching is used.

* **CMake Version Requirement**: `FetchContent` requires at least CMake 3.14. If you are using an older version of CMake, you might need to use other methods like `ExternalProject` or manual dependency management.

---

# Summary

* **`FetchContent`** allows you to download and build external dependencies as part of the CMake configuration process.

* It is a simple and effective way to manage dependencies directly within your project without needing the user to manually download or install them.

* You can fetch dependencies from Git repositories, tarballs, or other sources.

* Use `FetchContent_Declare()` to declare the external content and `FetchContent_MakeAvailable()` to download, extract, and make it available for your project.

* This approach ensures that your project always has the right version of external dependencies and integrates them seamlessly during the build process.

