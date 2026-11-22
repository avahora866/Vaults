---
tags:
  - FORMAT
  - C
---
Absolutely. Let's break down how to write a **basic `CMakeLists.txt`** file that CMake can use to configure and build a simple C++ project.

---

# Scenario

Let’s say you have a simple C++ program like this:

```
project/
├── main.cpp
└── CMakeLists.txt
```

And `main.cpp` looks like this:

```c
include <iostream>

int main() {
    std::cout << "Hello, CMake!" << std::endl;
    return 0;
}
```

---

# A Basic `CMakeLists.txt`

```
cmake_minimum_required(VERSION 3.10)

project(MyProject)

add_executable(MyApp main.cpp)
```

---

# Explanation

Let’s go through each line:

## `cmake_minimum_required(VERSION 3.10)`

* This sets the minimum version of CMake required to build your project.

* It ensures compatibility if your project uses features introduced in that version.

## `project(MyProject)`

* Defines the name of your project.

* This also sets up default variables like `PROJECT_NAME`.

## `add_executable(MyApp main.cpp)`

* This tells CMake to build an executable named `MyApp` using the source file `main.cpp`.

* If you had multiple source files, you'd list them all here:  
   `add_executable(MyApp main.cpp utils.cpp another_file.cpp)`

---

# Building the Project with CMake

Once you have this file and your `main.cpp`, do the following in the terminal:

```
cmake -S . -B build
cmake --build build
```

This will create a `build` directory, run the configure phase, and then build your `MyApp` executable.

---

# Adding More Features (Optional Basics)

You can expand this file to include:

## Set C++ standard:

```
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

## Include directories:

```
include_directories(include)
```

## Add a library:

```
add_library(MyLib mylib.cpp)
target_link_libraries(MyApp PRIVATE MyLib)
```

---

# Final Example with C++ Standard

```
cmake_minimum_required(VERSION 3.10)

project(MyProject)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_executable(MyApp main.cpp)
```

---

Let me know if you'd like to build a project with multiple files or link external libraries.

