---
tags:
  - Maven
  - "#LEAF"
---
# Basic concepts
## Maven POM
Maven POM is an XML file that contains information and configurations (about the project) that are used by Maven to import dependencies and build the project.

## Maven BOM
BOM stands for Bill Of Materials. A BOM is a special kind of POM that is used to control the versions of a project’s dependencies and provide a central place to define and update those versions.

BOM provides the flexibility to add a dependency to our module without worrying about the version that we should depend on.
# Transitive Dependencies
Maven can discover the libraries that are needed by our own dependencies in our pom.xml and includes them automatically. There’s no limit to the number of dependency levels that the libraries are gathered from.

The conflict here comes when 2 dependencies refer to different versions of a specific artifact. Which one will be included by Maven?

The answer here is the “nearest definition”. This means that the version used will be the closest one to our project in the tree of dependencies. This is called dependency mediation.

Let’s see the following example to clarify the dependency mediation:
`A -> B -> C -> D 1.4  and  A -> E -> D 1.0`
**This example shows that project A depends on B and E. B and E have their own dependencies which encounter different versions of the D artifact. Artifact D 1.0 will be used in the build of A project because the path through E is shorter.**

There are different techniques to determine which version of the artifacts should be included:

We can always guarantee a version by declaring it explicitly in our project’s POM. For instance, to guarantee that D 1.4 is used, we should add it explicitly as a dependency in the pom.xml file.
We can use the Dependency Management section to control artifact versions as we will explain later in this article.

# Dependecy Management
Simply put, Dependency Management is a mechanism to centralize the dependency information.

When we have a set of projects that inherit a common parent, we can put all dependency information in a shared POM file called BOM.

Following is an example of how to write a BOM file:

```
<project ...>
	
    <modelVersion>4.0.0</modelVersion>
    <groupId>baeldung</groupId>
    <artifactId>Baeldung-BOM</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>BaelDung-BOM</name>
    <description>parent pom</description>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>test</groupId>
                <artifactId>a</artifactId>
                <version>1.2</version>
            </dependency>
            <dependency>
                <groupId>test</groupId>
                <artifactId>b</artifactId>
                <version>1.0</version>
                <scope>compile</scope>
            </dependency>
            <dependency>
                <groupId>test</groupId>
                <artifactId>c</artifactId>
                <version>1.0</version>
                <scope>compile</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

As we can see, the BOM is a normal POM file with a dependencyManagement section where we can include all an artifact’s information and versions.

# Using the BOM File
There are 2 ways to use the previous BOM file in our project and then we will be ready to declare our dependencies without having to worry about version numbers.

We can inherit from the parent:
```
<project ...>
    <modelVersion>4.0.0</modelVersion>
    <groupId>baeldung</groupId>
    <artifactId>Test</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>Test</name>
    <parent>
        <groupId>baeldung</groupId>
        <artifactId>Baeldung-BOM</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
</project>
```

As we can see our project Test inherits the Baeldung-BOM.

We can also import the BOM.

In larger projects, the approach of inheritance is not efficient because the project can inherit only a single parent. Importing is the alternative as we can import as many BOMs as we need.

Let’s see how we can import a BOM file into our project POM:

```
<project ...>
    <modelVersion>4.0.0</modelVersion>
    <groupId>baeldung</groupId>
    <artifactId>Test</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>Test</name>
    
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>baeldung</groupId>
                <artifactId>Baeldung-BOM</artifactId>
                <version>0.0.1-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```
The order of precedence of the artifact’s version is:

The version of the artifact’s direct declaration in our project pom
The version of the artifact in the parent project
The version in the imported pom, taking into consideration the order of importing files
dependency mediation
We can overwrite the artifact’s version by explicitly defining the artifact in our project’s pom with the desired version
If the same artifact is defined with different versions in 2 imported BOMs, then the version in the BOM file that was declared first will win
# Spring BOM
We may find that a third-party library, or another Spring project, pulls in a transitive dependency to an older release. If we forget to explicitly declare a direct dependency, unexpected issues can arise.

To overcome such problems, Maven supports the concept of BOM dependency.

We can import the spring-framework-bom in our dependencyManagement section to ensure that all Spring dependencies are at the same version:
```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-framework-bom</artifactId>
            <version>5.3.27</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
We don’t need to specify the version attribute when we use the Spring artifacts as in the following example:
```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
    </dependency>
<dependencies>
```







