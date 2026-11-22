---
tags:
  - Maven
  - "#LEAF"
---
A multi-module project is built from an aggregator POM that manages a group of submodules. In most cases, the aggregator is located in the project’s root directory and must have packaging of type *pom*.

The submodules are regular Maven projects, and they can be built separately or through the aggregator POM.

By building the project through the aggregator POM, each project that has a packaging type different from *pom* will result in a built archive file.

Maven multi-module projects are useful during the build step when we have multiple projects that are related together that are at a release point. We could have to build each project at a time instead of the multi-maven project approach where we would simply have to run 1 command.

Sometimes projects require diffrent maven build steps which can all occur using the maven multi module build step.

# Parent Pom

Maven supports inheritance in a way that each pom.xml file has the implicit parent POM. It’s called Super POM and can be located in the Maven binaries. These two files are merged by Maven and form the Effective POM.

We can create our own pom.xml file, which will serve us as the parent project. Then we can include in it all configuration with dependencies, and set it as the parent of our child modules, so they’ll inherit from it.

Besides the inheritance, Maven provides the notion of aggregation. A parent POM that leverages this functionality is called an aggregate POM. Basically, this kind of POM declares its modules explicitly in its pom.xml file.

# Submodules

Submodules, or subprojects, are regular Maven projects that inherit from the parent POM. As we already know, inheritance lets us share the configuration and dependencies with submodules. However, if we’d like to build or release our project in one shot, we have to declare our submodules explicitly in the parent POM. Ultimately, our parent POM will be the parent, as well as the aggregate POM.