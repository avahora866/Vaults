---
tags:
  - FLASHCARD
  - Java
---
In Java programming, the **classpath** is a critical concept that tells the Java Virtual Machine (JVM) and Java compiler where to find the classes (compiled `.class` files) and other resources needed for a Java program to run.

- **Definition**: The classpath is essentially a set of directories, `.jar` files (Java Archive), or `.zip` files that contain classes and other resources your Java application needs.
- **Purpose**: When you run or compile a Java application, the JVM uses the classpath to locate the classes referenced in your code. If a required class isn't found in the classpath, the JVM will throw a `ClassNotFoundException` or similar error.

# Intellij
https://www.jetbrains.com/help/idea/compiling-applications.html#package_into_jar
## Create an artifact configuration for the JAR

1. In the main menu, go to File | Project Structure CtrlAltShift0S and click Artifacts.
2. Click the add button, point to JAR, and select From modules with dependencies.
3. To the right of the Main Class field, click the browse button for example, `HelloWorld` (`com.example.helloworld`)).
    IntelliJ IDEA creates the artifact configuration and shows its settings in the right-hand part of the Project Structure dialog.
4. Apply the changes and close the dialog.

## Build the JAR artifact﻿
1. In the main menu, go to Build | Build Artifacts.
2. Point to the created .jar (`HelloWorld:jar`) and select Build.
    If you now look at the `out/artifacts` folder, you'll find your `.jar` file there.

## Use .jar in your project
1. Project Structure -> Libraries -> add your jar