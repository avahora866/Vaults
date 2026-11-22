---
tags:
  - Maven
  - "#TRUNK"
---
# Introduction

**Maven is a plugin execution framework and is built around the premise of lifecycles, which are a set of phases, which are made up of plugin goals.**

[Maven Build Lifecycles](../../../Software%20Development/Build%20Tools/Maven/Maven%20Build%20Lifecycles.md)

# Why Use Maven?

The key features of Maven are:

- **simple project setup that follows best practices:** Maven tries to avoid as much configuration as possible, by supplying project templates (named archetypes)
- **dependency management:** it includes automatic updating, downloading and validating the compatibility, as well as reporting the dependency closures (known also as transitive dependencies)
- **isolation between project dependencies and plugins:** with Maven, project dependencies are retrieved from the dependency repositories while any plugin's dependencies are retrieved from the plugin repositories, resulting in fewer conflicts when plugins start to download additional dependencies
- **central repository system:** project dependencies can be loaded from the local file system or public repositories, such as **[Maven Central](https://search.maven.org/classic/)**

In this section, we will use the command line functionality of Maven to create a Java project.

# Project Object Model

**[Project Object Model](../../../Software%20Development/Build%20Tools/Maven/Project%20Object%20Model.md)**

# Your first Maven Project

## Generating a Simple Java Project

In order to build a simple Java project, let's run the following command:

```
mvn archetype:generate \
  -DgroupId=com.baeldung \
  -DartifactId=baeldung \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DarchetypeVersion=1.4 \
  -DinteractiveMode=false
```

The `groupId` is a parameter indicating the group or individual that created a project, which is often a reversed company domain name. The `artifactId` is the base package name used in the project, and we use the standard `archetype`. Here we are using the latest archetype version to ensure our project is created with updated and latest structure.  
Since we didn't specify the version and the packaging type, these will be set to default values — the version will be set to 1.0-SNAPSHOT, and the packaging will be jar by default.  
If you don't know which parameters to provide, you can always specify interactiveMode=true, so that Maven asks for all the required parameters.  
After the command completes, we have a Java project containing an App.java class, which is just a simple “Hello World” program, in the src/main/java folder.  
We also have an example test class in src/test/java. The pom.xml of this project will look similar to this:

```XML
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.baeldung</groupId>
    <artifactId>baeldung</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>baeldung</name>
    <url>http://www.example.com</url>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

As you can see, the JUnit dependency is provided by default.

## Compiling and Packaging a Project

The next step is to compile the project:

```
mvn compileCopy
```

Maven will run through all lifecycle phases needed by the compile phase to build the project's sources. If you want to run only the test phase, you can use:

```
mvn testCopy
```

Now let's invoke the package phase, which will produce the compiled archive jar file:

```
mvn package
```

## Executing an Application

Finally, we are going to execute our Java project with the exec-maven-plugin. Let's configure the necessary plugins in the pom.xml:

```xml
<build>
    <sourceDirectory>src</sourceDirectory>
    <plugins>
        <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>3.0.0</version>
            <configuration>
                <mainClass>com.baeldung.java.App</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
```

The first plugin, [maven-compiler-plugin](https://search.maven.org/classic/#search%7Cga%7C1%7Ca%3A%22maven-compiler-plugin%22), is responsible for compiling the source code using Java version 1.8. The [exec-maven-plugin](https://search.maven.org/classic/#search%7Cga%7C1%7Ca%3A%22exec-maven-plugin%22) searches for the mainClass in our project.

To execute the application, we run the following command:

```
mvn exec:java
```

# Other features of maven

- [Maven wrapper](../../../Software%20Development/Build%20Tools/Maven/Maven%20wrapper.md)
- [Multi-Module Projects](../../../Software%20Development/Build%20Tools/Maven/Multi-Module%20Projects.md)

# Important terms

- Archetype: maven archetypes projects that can be used asa template for when developers need to quickly spin up a new project. It can define classes in the project, required properties in the pom.xml and other aspects. The maven archetype project packaging element in the pom.xml is set to maven-archetype and once maven installed is saved in the file archetype-catolog.xml in the Maven local repository.