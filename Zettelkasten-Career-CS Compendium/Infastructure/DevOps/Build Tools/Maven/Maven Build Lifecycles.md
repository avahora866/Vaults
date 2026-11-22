---
tags:
  - Maven
  - "#SECONDARY_BRANCH"
---
# Maven Build Lifecycles

There are 3 built-in lifecycles:

- clean
- deploy
- site

## Lifecycle Phases

The following list shows the deploy lifecycle phases:

- validate – checks the correctness of the project
- compile – compiles the provided source code into binary artifacts
- test – executes unit tests
- package – packages compiled code into an archive file
- integration-test – executes additional tests, which require the packaging
- verify – checks if the package is valid
- install – installs the package file into the local Maven repository
- deploy – deploys the package file to a remote server or repository

## Plugins and Goals

A Maven plugin is a collection of one or more goals. Goals are executed in phases, which helps to determine the order in which the goals are executed.  
The rich list of plugins that are officially supported by Maven is available [here](https://maven.apache.org/plugins/). There is also an interesting article on how to [build an executable JAR on Baeldung](https://www.baeldung.com/executable-jar-with-maven) using various plugins.  
To gain a better understanding of which goals are run in which phases by default, take a look at the [default Maven lifecycle bindings](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings).  
To go through any one of the above phases, we just have to call one command:

```
mvn <phase>
```

For example, mvn clean install will remove the previously created jar/war/zip files and compiled classes (clean) and execute all the phases necessary to install the new archive (install).  
Please note that goals provided by plugins can be associated with different phases of the lifecycle.