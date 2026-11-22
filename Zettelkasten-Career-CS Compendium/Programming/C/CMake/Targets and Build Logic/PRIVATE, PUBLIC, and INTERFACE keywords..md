---
tags:
  - FORMAT
  - C
---
Certainly\! In CMake, the keywords **PRIVATE**, **PUBLIC**, and **INTERFACE** are used to define the visibility of **target properties**, especially when dealing with **dependencies** (such as libraries or other targets). These keywords control **how properties, include directories, compile definitions, and libraries** are propagated between targets.

Let’s break down each one and explain when and why to use them.

---

# PRIVATE

**What it means**:  
 When you use **PRIVATE**, you are specifying that the property or setting applies **only to the target itself** and **not to anything that links to the target**.

**Use case**:

* Use **PRIVATE** when you want a property or dependency to be internal to the target and not propagate to other targets that depend on it.

**Common scenarios**:

* You’re adding **compile flags** that should only apply to the target itself.

* You want to link a **library** that is only needed at the build stage, not by other targets that depend on this one.

**Example**:

```
target_include_directories(MyApp PRIVATE ${CMAKE_SOURCE_DIR}/include)
```

Here:

* The `MyApp` target will include headers from `${CMAKE_SOURCE_DIR}/include`, but this include directory will **not** be propagated to any targets that depend on `MyApp`.

---

# PUBLIC

**What it means**:  
 When you use **PUBLIC**, the property or setting applies **both to the target itself and to any target that links to it**.

**Use case**:

* Use **PUBLIC** when you want to specify a property that should **affect both** the target you're working on and any targets that **depend on** it.

**Common scenarios**:

* You want to set **include directories** that should be accessible both within the target and to other targets that link to it.

* You’re linking a **library** that should be passed along to anything that links to the target.

**Example**:

```
target_include_directories(MyApp PUBLIC ${CMAKE_SOURCE_DIR}/include)
```

Here:

* The `MyApp` target will include headers from `${CMAKE_SOURCE_DIR}/include`, and any target that links to `MyApp` will also get these include directories automatically.

Another common example is when linking libraries:

```
target_link_libraries(MyApp PUBLIC MyLib)
```

In this case:

* `MyApp` is linked to `MyLib`, and any target that links to `MyApp` will automatically also link to `MyLib`.

---

# INTERFACE

**What it means**:  
 **INTERFACE** is a special keyword used to specify that a property or setting applies **only to targets that link to the current target**, but **not** to the target itself.

**Use case**:

* Use **INTERFACE** when you want to specify settings or dependencies that are **only relevant to consumers** of your target (i.e., other targets that link to this one), but the target itself doesn’t need them.

**Common scenarios**:

* You want to define **header-only libraries** (libraries that don’t have source files) where only the **include directories** or **compile definitions** are relevant to targets that link to it.

* You have a library that **doesn’t need to be compiled** but provides essential headers or compile flags.

**Example**:

```
add_library(MyHeaderOnlyLib INTERFACE)
target_include_directories(MyHeaderOnlyLib INTERFACE ${CMAKE_SOURCE_DIR}/include)
```

Here:

* `MyHeaderOnlyLib` is an **interface library**, which means it doesn’t generate any binary code or compiled files.

* The `INTERFACE` keyword ensures that only targets that **link to** `MyHeaderOnlyLib` will get the include directories.

---

# Summary of Differences

| Keyword | Applies To | Propagates To Linked Targets? | Use Case |
| ----- | ----- | ----- | ----- |
| **PRIVATE** | Only the target itself | No | For internal settings or dependencies that shouldn’t propagate |
| **PUBLIC** | The target and linked targets | Yes | For settings or dependencies that affect both the target and its consumers |
| **INTERFACE** | Only linked targets | Yes | For settings or dependencies that only affect consumers (header-only libraries) |

---

# More Detailed Examples

## Example 1: Linking Libraries with Different Visibility

```
add_library(MyLib STATIC mylib.cpp)
target_include_directories(MyLib PRIVATE ${CMAKE_SOURCE_DIR}/priv_includes)
target_include_directories(MyLib PUBLIC ${CMAKE_SOURCE_DIR}/pub_includes)

add_executable(MyApp main.cpp)
target_link_libraries(MyApp PUBLIC MyLib)
```

*   
  **`MyLib`**:

  * **PRIVATE**: The include directory `${CMAKE_SOURCE_DIR}/priv_includes` is only used by `MyLib` itself.

  * **PUBLIC**: The include directory `${CMAKE_SOURCE_DIR}/pub_includes` is used by both `MyLib` and any target that links to `MyLib`.

* **`MyApp`**:

  * Since `MyApp` links to `MyLib` with `PUBLIC`, `MyApp` will inherit the include directory `${CMAKE_SOURCE_DIR}/pub_includes`.

## Example 2: Header-Only Library with `INTERFACE`

```
add_library(MyHeaderOnlyLib INTERFACE)
target_include_directories(MyHeaderOnlyLib INTERFACE ${CMAKE_SOURCE_DIR}/include)

add_executable(MyApp main.cpp)
target_link_libraries(MyApp MyHeaderOnlyLib)
```

*   
  **`MyHeaderOnlyLib`**:

  * **INTERFACE**: It doesn’t have any compiled code but provides an interface, specifically the include directories.

* **`MyApp`**:

  * Links to `MyHeaderOnlyLib` and thus gets the include directory `${CMAKE_SOURCE_DIR}/include`, even though `MyHeaderOnlyLib` itself doesn't need to compile anything.

---

# Key Takeaways:

* **PRIVATE**: The property applies only to the target itself.

* **PUBLIC**: The property applies to both the target and any other target that links to it.

* **INTERFACE**: The property applies only to targets that link to it, not to the target itself.

These keywords give you fine-grained control over how properties, dependencies, and settings are propagated through your project’s targets. Understanding how to use them helps ensure your build system behaves correctly and efficiently, especially in larger, more complex projects.

Let me know if you need more examples or clarification\!

