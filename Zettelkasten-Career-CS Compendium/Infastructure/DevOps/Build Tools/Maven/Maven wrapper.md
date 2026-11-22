---
tags:
  - Maven
  - "#LEAF"
---
Maven wrappers are used to run maven commands on an environment where there is no maven installed and no maven path set.

```
mvn -N wrapper:wrapper
```

&nbsp;Above command is used to create the wrapper we can also add a -Dmaven=&lt;version number&gt; to specify the version.

After executing the goal, we’ll have more files and directories in the project:

- *mvnw*: it’s an executable Unix shell script used in place of a fully installed Maven
- *mvnw.cmd*: it’s the Batch version of the above script
- *mvn*: the hidden folder that holds the Maven Wrapper Java library and its properties file

The wrapper should work with different operating systems such as:

- Linux
- OSX
- Windows
- Solaris

After that, we can run our goals like this for the Unix system:

```
./mvnw clean install
```

And the following command for Windows:

```
mvnw.cmd clean install
```

**If we don’t have the specified Maven in the wrapper properties, it’ll be downloaded and installed in the folder *$USER_HOME/.m2/wrapper/dists* of the system.**

Note: we use the executable *mvnw* in place of *mvn,* which stands now as the Maven command line program.

Now that we know what Maven wrapper is, let’s answer one of the common questions: should *mvnw* files be added to our projects?

The short answer is no. ***mvnw* files are not necessarily part of our projects**. However, including them could be beneficial. For example, it will allow anyone cloning our project to build it without installing Maven.