---
tags:
  - FORMAT
  - C
---
# Using `target_link_libraries()` to Link Against External Packages in CMake

In CMake, the `target_link_libraries()` command is used to specify which libraries or external packages your target (e.g., an executable or a library) should link against. This is a crucial step in the build process, as it tells the compiler and linker which libraries should be included when building your project.

Linking external packages with `target_link_libraries()` ensures that the necessary libraries are included and linked properly, whether they are system libraries, libraries you’ve built yourself, or external dependencies like third-party libraries.

---

# Syntax of `target_link_libraries()`

The basic syntax of `target_link_libraries()` is as follows:

```
target_link_libraries(<target> <library> [<library> ...])
```

*   
  **`<target>`**: The name of your target (e.g., the executable or library you're building).

* **`<library>`**: The libraries you want to link your target to. These can be other targets, system libraries, or external dependencies.

You can also specify additional options that control how the libraries are linked to the target (e.g., `PUBLIC`, `PRIVATE`, `INTERFACE`).

---

# Linking Against External Libraries

When you are linking against external libraries, these libraries may come from various sources:

* **System libraries**: Libraries installed on your system (e.g., `pthread`, `dl`, etc.).

* **CMake targets from other projects**: Libraries that are part of your project or external libraries discovered using CMake (`find_package()`, `FetchContent()`, etc.).

* **Static or shared libraries**: You can link against both static libraries (`.a`, `.lib`) and shared libraries (`.so`, `.dll`).

# Example 1: Linking Against a System Library (e.g., `pthread`)

If you're linking against a system library like `pthread`, you would use `target_link_libraries()` like this:

```
add_executable(MyApp main.cpp)
target_link_libraries(MyApp PRIVATE pthread)
```

This tells CMake to link the `pthread` library to the `MyApp` target. The `PRIVATE` keyword indicates that the `pthread` library is needed only by `MyApp` and does not propagate to other targets that depend on `MyApp`.

---

# Example 2: Linking Against a Package Found with `find_package()`

If you’re using an external package that CMake can find using `find_package()`, you would link against it as follows:

## Example: Linking Against `Boost`

```
 Find the Boost package
find_package(Boost REQUIRED)

 Create an executable
add_executable(MyApp main.cpp)

 Link Boost to your target
target_link_libraries(MyApp PRIVATE Boost::Boost)
```

In this case:

* `find_package(Boost REQUIRED)` locates the Boost package (you can specify the version or other criteria).

* `Boost::Boost` is the target that is created by `find_package()` when Boost is found and properly configured.

* `target_link_libraries(MyApp PRIVATE Boost::Boost)` links the Boost library to your target `MyApp`.

---

# Example 3: Linking Against a Custom or External Library Using `find_package()` and `target_link_libraries()`

If you’re using a library that is external or you’ve written yourself, you might use the `find_package()` command to locate it, and then link it with `target_link_libraries()`.

## Example: Using `find_package()` for a Custom Library

```
 Find the custom library package
find_package(MyCustomLibrary REQUIRED)

 Create an executable
add_executable(MyApp main.cpp)

 Link the custom library
target_link_libraries(MyApp PRIVATE MyCustomLibrary::MyCustomLibrary)
```

In this case:

* `find_package(MyCustomLibrary REQUIRED)` looks for the `MyCustomLibrary` package, which could be a third-party or locally available library.

* `MyCustomLibrary::MyCustomLibrary` is the imported target created by `find_package()`.

* `target_link_libraries(MyApp PRIVATE MyCustomLibrary::MyCustomLibrary)` links your executable `MyApp` with `MyCustomLibrary`.

---

# Example 4: Linking Against a Library Built in the Same Project

If you have multiple targets in your project and you want to link an executable with a library defined within the same project, you can do it as follows:

```
 Define a library
add_library(MyLibrary STATIC my_library.cpp)

 Define an executable
add_executable(MyApp main.cpp)

 Link the executable to the library
target_link_libraries(MyApp PRIVATE MyLibrary)
```

In this case:

* `add_library(MyLibrary STATIC my_library.cpp)` creates a static library `MyLibrary`.

* `target_link_libraries(MyApp PRIVATE MyLibrary)` links the `MyApp` executable to `MyLibrary`.

---

# Using `PRIVATE`, `PUBLIC`, and `INTERFACE` Keywords

The keywords `PRIVATE`, `PUBLIC`, and `INTERFACE` determine the scope and propagation of the library linkage.

* **`PRIVATE`**: The library is linked only for the specified target, and does not propagate to any other targets that depend on it. This is the most common case.

```
target_link_libraries(MyApp PRIVATE SomeLibrary)
```

*   
  Here, `SomeLibrary` is linked only to `MyApp`.

* **`PUBLIC`**: The library is linked to the specified target, and any targets that depend on this target will also link to the library.

```
target_link_libraries(MyApp PUBLIC SomeLibrary)
```

*   
  In this case, `SomeLibrary` is linked to `MyApp`, and any target that links against `MyApp` will automatically also link against `SomeLibrary`.

* **`INTERFACE`**: The library is not linked to the specified target but is required for any targets that depend on it. This is typically used for header-only libraries or transitive dependencies.

```
target_link_libraries(MyApp INTERFACE SomeHeaderOnlyLibrary)
```

# Example: Combining `PRIVATE`, `PUBLIC`, and `INTERFACE`

In more complex scenarios, you may want to specify different types of linkage for different targets. Here's an example:

```
add_library(MyLibrary STATIC my_library.cpp)

 MyLibrary uses some external dependencies (like Boost)
target_link_libraries(MyLibrary PUBLIC Boost::Boost)

 MyApp uses MyLibrary, so it will also link Boost
add_executable(MyApp main.cpp)
target_link_libraries(MyApp PRIVATE MyLibrary)
```

In this example:

* `MyLibrary` is linked with `Boost::Boost` via `PUBLIC`, meaning any target that links `MyLibrary` will also link Boost.

* `MyApp` links `MyLibrary` via `PRIVATE`, so it doesn't propagate the Boost dependency further.

---

# Using `target_link_libraries()` with Static and Shared Libraries

You can link both static and shared libraries in the same way. However, when building shared libraries, you may need to ensure that the linker is aware of both the shared and static dependencies.

For example, if you are creating a shared library and want to link it with both static and shared libraries:

```
add_library(MySharedLibrary SHARED my_shared_library.cpp)
target_link_libraries(MySharedLibrary PRIVATE SomeStaticLibrary SomeSharedLibrary)
```

Here, `SomeStaticLibrary` is a static library, and `SomeSharedLibrary` is a shared library. Both are linked to `MySharedLibrary` as private dependencies.

---

# Summary

* `target_link_libraries()` is used in CMake to link a target (e.g., an executable or library) to other libraries or external packages.

* You can link system libraries, libraries found via `find_package()`, libraries from external projects, or libraries within your own project.

* You control the visibility and propagation of the linked libraries using the `PRIVATE`, `PUBLIC`, and `INTERFACE` keywords.

  * **`PRIVATE`**: Links the library to the target without affecting others.

  * **`PUBLIC`**: Links the library to the target and propagates the dependency to dependent targets.

  * **`INTERFACE`**: Requires the library for dependent targets but does not link it to the current target.

* By using `target_link_libraries()` effectively, you ensure that your project is properly linked with all the necessary external and internal dependencies.

