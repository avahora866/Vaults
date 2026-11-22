---
tags:
  - Maven
  - "#LEAF"
---
The configuration of a Maven project is done via a Project Object Model (POM), represented by a pom.xml file. The POM describes the project, manages dependencies, and configures plugins for building the software.

The POM also defines the relationships among modules of multi-module projects. Let's look at the basic structure of a typical POM file:

```
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.baeldung</groupId>
    <artifactId>baeldung</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>com.baeldung</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.8.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
            //...
            </plugin>
        </plugins>
    </build>
</project>
```

Let's take a closer look at these constructs.

# Project Identifiers

Maven uses a set of identifiers, also called coordinates, to uniquely identify a project and specify how the project artifact should be packaged:

- `groupId` – a unique base name of the company or group that created the project
- `artifactId` – a unique name of the project
- `version` – a version of the project
- `packaging` – a packaging method (e.g. WAR/JAR/ZIP)

The first three of these (`groupId:artifactId:version`) combine to form the unique identifier and are the mechanism by which you specify which versions of external libraries (e.g. JARs) your project will use.

# Dependencies

These external libraries that a project uses are called dependencies. The dependency management feature in Maven ensures the automatic download of those libraries from a central repository, so you don't have to store them locally.

This is a key feature of Maven and provides the following benefits:

- uses less storage by significantly reducing the number of downloads off remote repositories
- makes checking out a project quicker
- provides an effective platform for exchanging binary artifacts within your organization and beyond without the need for building artifacts from source every time

In order to declare a dependency on an external library, you need to provide the `groupId`, `artifactId`, and `version` of the library. Let's take a look at an example:

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.16</version>
</dependency>
```

As Maven processes the dependencies, it will download the Spring Core library into your local Maven repository.

## Types of dependencies

- Direct Dependeiencies: These are dependeices that are explicitly included in the pom.xml in the &lt;dependeies&gt; section
- Transitive Dependencies: A project (Project B) that has been included in another projects (Project A) pom.xml can have its own pom.xml. The dependencies in Project B are considered as transitive dependencies in the context of Project A. When maven is pulling the direct dependencies of Project A (including Project B) it will also pull in project B's transitive dependencies

&nbsp;

## Repositories

A repository in Maven is used to hold build artifacts and dependencies of varying types. The default local repository is located in the .m2/repository folder under the home directory of the user.  
f an artifact or a plugin is available in the local repository, Maven uses it. Otherwise, it is downloaded from a central repository and stored in the local repository. The default central repository is [Maven Central](https://search.maven.org/classic/#search%7Cga%7C1%7Ccentra).  
Some libraries, such as the JBoss server, are not available at the central repository but are available at an alternate repository. For those libraries, you need to provide the URL to the alternate repository inside the pom.xml file:

```
<repositories>
    <repository>
        <id>JBoss repository</id>
        <url>http://repository.jboss.org/nexus/content/groups/public/</url>
    </repository>
</repositories>
```

Please note that you can use multiple repositories in your projects.

# Properties

Custom properties can help to make your pom.xml file easier to read and maintain. In the classic use case, you would use custom properties to define versions for your project's dependencies.  
Maven properties are value-placeholders and are accessible anywhere within a pom.xml by using the notation `${name}`, where name is the property.  
Let's see an example:

```
<properties>
    <spring.version>5.3.16</spring.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

Now if you want to upgrade Spring to a newer version, you only have to change the value inside the `<spring.version>` property tag and all the dependencies using that property in their `<version>` tags will be updated.  
Properties are also often used to define build path variables:

```
<properties>
    <project.build.folder>${project.build.directory}/tmp/</project.build.folder>
</properties>

<plugin>
    //...
    <outputDirectory>${project.resources.build.folder}</outputDirectory>
    //...
</plugin>
```

# Build

The build section is also a very important section of the Maven POM. It provides information about the default Maven goal, the directory for the compiled project, and the final name of the application. The default build section looks like this:

```
<build>
    <defaultGoal>install</defaultGoal>
    <directory>${basedir}/target</directory>
    <finalName>${artifactId}-${version}</finalName>
    <filters>
      <filter>filters/filter1.properties</filter>
    </filters>
    //...
</build>
```

The default output folder for compiled artifacts is named target, and the final name of the packaged artifact consists of the artifactId and version, but you can change it at any time.

# Using Profiles

Another important feature of Maven is its support for profiles. A profile is basically a set of configuration values. By using profiles, you can customize the build for different environments such as Production/Test/Development:

```
<profiles>
    <profile>
        <id>production</id>
        <build>
            <plugins>
                <plugin>
                //...
                </plugin>
            </plugins>
        </build>
    </profile>
    <profile>
        <id>development</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <build>
            <plugins>
                <plugin>
                //...
                </plugin>
            </plugins>
        </build>
     </profile>
 </profiles>
```

As you can see in the example above, the default profile is set to development. If you want to run the production profile, you can use the following Maven command:

```
mvn clean install -Pproduction
```