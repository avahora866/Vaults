---
tags:
  - Java
---
# Enabling Logging[](https://www.baeldung.com/java-logging-intro#Logging)

All the logging frameworks discussed in the article share the notion of loggers, appenders, and layouts. Enabling logging inside the project follows three common steps:

1.  Adding needed libraries
2.  Configuration
3.  Placing log statements

The upcoming sections discuss the steps for each framework individually.

# Log4j 2

Log4j 2 is a new and improved version of the Log4j logging framework. The most compelling improvement is the possibility of asynchronous logging. Log4j 2 requires the following libraries:

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.6.1</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.6.1</version>
</dependency>
```

### Configuration[](https://www.baeldung.com/java-logging-intro#1-configuration)

Configuring Log4j 2 is based on the main configuration *log4j2.xml* file. The first thing to configure is the appender.

These determine where the log message will be routed. The destination can be a console, a file, a socket, etc.

Log4j 2 has many appenders for different purposes; you can find more information on the official [Log4j 2](https://logging.apache.org/log4j/2.x/manual/appenders.html) site.

Let’s take a look at a simple config example:

```xml
<Configuration status="debug" name="baeldung" packages="">
    <Appenders>
        <Console name="stdout" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} %p %m%n"/>
        </Console>
    </Appenders>
</Configuration>
```

You can set a name for each appender; for example, use name *console* instead of *stdout*.

Notice the *PatternLayout* element – this determines how the message should look like. In our example, the pattern is set based on the *pattern* param, where *%d* determines the date pattern, *%p* – the log level output, *%m* – output of the logged message, and *%n* – adds a new line symbol. More info about patterns can be found on the official [Log4j 2](https://logging.apache.org/log4j/2.x/manual/layouts.html) page.

Finally – **to enable an appender** (or multiple), you need to add it to the *&lt;Loggers&gt;* section:

```xml
<Loggers> 
    <Root level="error">
        <AppenderRef ref="STDOUT"/>
    </Root>
</Loggers>
```
## Logging to File
Sometimes, you’ll need to use logging to a file, so we will add fout logger to our configuration:

```xml
<Appenders>
    <File name="fout" fileName="baeldung.log" append="true">
        <PatternLayout>
            <Pattern>%d{yyyy-MM-dd HH:mm:ss} %-5p %m%n</Pattern>
        </PatternLayout>
    </File>
</Appenders>
```
The File appender has several parameters that can be configured:

file – determines the file name of the log file
append – The default value for this param is true, meaning that by default, a File appender will append to an existing file and not truncate it.
PatternLayout that was described in the previous example.
In order to enable File Appender, you need to add it to the `<Root> `section:
```xml
<Root level="INFO">
    <AppenderRef ref="stdout" />
    <AppenderRef ref="fout"/>
</Root>

```
## Asynchronous Logging
If you want to make your Log4j 2 asynchronous, you need to add the LMAX disruptor library to your pom.xml. LMAX disruptor is a [[Lock|lock]]-free inter-thread communication library.

Adding disruptor to pom.xml:

```xml
<dependency>
    <groupId>com.lmax</groupId>
    <artifactId>disruptor</artifactId>
    <version>3.3.4</version>
</dependency>
```
If you want to use LMAX disruptor, you need to use `<asyncRoot>` instead of `<Root>` in your configuration.

```xml
<AsyncRoot level="DEBUG">
    <AppenderRef ref="stdout" />
    <AppenderRef ref="fout"/>
</AsyncRoot>
```
Or you can enable asynchronous logging by setting the system property Log4jContextSelector to org.apache.logging.log4j.core.async.AsyncLoggerContextSelector.

You can, of course, read more about the configuration of the Log4j2 async logger and see some performance diagrams on the Log4j2 official page.

## Usage
The following is a simple example that demonstrates the use of Log4j for logging:

```java
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

public class Log4jExample {

    private static Logger logger = LogManager.getLogger(Log4jExample.class);

    public static void main(String[] args) {
        logger.debug("Debug log message");
        logger.info("Info log message");
        logger.error("Error log message");
    }
}
```
After running, the application will log the following messages to both the console and file named baeldung.log:

```
2016-06-16 17:02:13 INFO  Info log message
2016-06-16 17:02:13 ERROR Error log message
```
If you elevate the root log level to ERROR:
```xml
<level value="ERROR" />
```
The output will look like the following:
```
2016-06-16 17:02:13 ERROR Error log message
```

As you can see, changing the log level to the upper parameter causes the messages with lower log levels not to be printed to appenders.

Method logger.error can also be used to log an exception that occurred:
```
try {
    // Here some exception can be thrown
} catch (Exception e) {
    logger.error("Error log message", throwable);
}
```

## Package Level Configuration
Let’s say you need to show messages with the log level TRACE – for example, from a specific package such as com.baeldung.log4j2:
```
logger.trace("Trace log message");
```

For all other packages, you want to continue logging only INFO messages.
Keep in mind that TRACE is lower than the root log level INFO that we specified in the configuration.
To enable logging only for one of the packages, you need to add the following section before `<Root>` to your log4j2.xml:

```xml
<Logger name="com.baeldung.log4j2" level="debug">
    <AppenderRef ref="stdout"/>
</Logger>
```
It will enable logging for the com.baeldung.log4j package and your output will look like this:

```
2016-06-16 17:02:13 TRACE Trace log message
2016-06-16 17:02:13 DEBUG Debug log message
2016-06-16 17:02:13 INFO  Info log message
2016-06-16 17:02:13 ERROR Error log message
```
# Logback
Logback is meant to be an improved version of Log4j, developed by the same developer who made Log4j.

Logback also has many more features than Log4j, with many of them being introduced into Log4j 2 as well. Here’s a quick look at all of the advantages of Logback on the official site.

Let’s start by adding the following dependency to the pom.xml:
```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.6</version>
</dependency>
```
This dependency will transitively pull in another two dependencies, the logback-core and slf4j-api. Note that the latest version of Logback can be found here.
## Configuration
Let’s now have a look at a Logback configuration example:
```xml
<configuration>
  # Console appender
  <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      # Pattern of log message for console appender
      <Pattern>%d{yyyy-MM-dd HH:mm:ss} %-5p %m%n</Pattern>
    </layout>
  </appender>

  # File appender
  <appender name="fout" class="ch.qos.logback.core.FileAppender">
    <file>baeldung.log</file>
    <append>false</append>
    <encoder>
      # Pattern of log message for file appender
      <pattern>%d{yyyy-MM-dd HH:mm:ss} %-5p %m%n</pattern>
    </encoder>
  </appender>

  # Override log level for specified package
  <logger name="com.baeldung.log4j" level="TRACE"/>

  <root level="INFO">
    <appender-ref ref="stdout" />
    <appender-ref ref="fout" />
  </root>
</configuration>
```

Logback uses SLF4J as an interface, so you need to import SLF4J’s Logger and LoggerFactory.

## SLF4J
SLF4J provides a common interface and abstraction for most of the Java logging frameworks. It acts as a facade and provides a standardized API for accessing the underlying features of the logging framework.

Logback uses SLF4J as a native API for its functionality. Following is the example using Logback logging:
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Log4jExample {

    private static Logger logger = LoggerFactory.getLogger(Log4jExample.class);

    public static void main(String[] args) {
        logger.debug("Debug log message");
        logger.info("Info log message");
        logger.error("Error log message");
    }
}
```
The output will remain the same as in previous examples.
# Log4J
Finally, let’s have a look at the venerable Log4j logging framework.

At this point, it’s outdated, but worth discussing as it lays the foundation for its more modern successors.

Many of the configuration details match those discussed in the Log4j 2 section
## Configuration
First of all, you need to add the Log4j library to your projects pom.xml:
```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```
Let’s take a look at a complete example of a simple Log4j configuration with only one console appender:
```xml
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd" >
<log4j:configuration debug="false">

    <!--Console appender-->
    <appender name="stdout" class="org.apache.log4j.ConsoleAppender">
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" 
              value="%d{yyyy-MM-dd HH:mm:ss} %p %m%n" />
        </layout>
    </appender>

    <root>
        <level value="INFO" />
        <appender-ref ref="stdout" />
    </root>

</log4j:configuration>
```
`<log4j:configuration debug=”false”>` is the open tag of the whole configuration which has one property – debug. It determines whether you want to add Log4j debug information to logs.
## Usage
After you have added the Log4j library and configuration, you can use the logger in your code. Let’s take a look at a simple example:
```java
import org.apache.log4j.Logger;

public class Log4jExample {
    private static Logger logger = Logger.getLogger(Log4jExample.class);

    public static void main(String[] args) {
        logger.debug("Debug log message");
        logger.info("Info log message");
        logger.error("Error log message");
    }
}
```









