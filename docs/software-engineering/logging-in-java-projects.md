# Logging in Java projects

"Logging in Java" is quite the rabbit hole, that I've been down many times, and whilst the documentation & knowledge is
available, it can be a little hard to find.

From my experience so far, here's how it works (as of 2024):

## Logging Considerations

Before you go write `System.out.println` or directly using
the [Java Logging API](https://docs.oracle.com/en/java/javase/22/docs/api/java.logging/module-summary.html) and
shouting "I'm done!", there's a bunch of things to consider when you want to create a log message:

- What information do you want with your log message?
	- Timestamp?
	- Class that logged the message?
	- Thread the log was on?
	- Logging level?
	- Extra data about what was happening at the time?
	- Custom labels/markers?
	- etc.
- How do you want that log to be formatted?
	- Plain string?
	- Custom format?
	- JSON object?
	- XML object?
- Where do you want the log to be sent?
	- Console output?
	- A file?
	- Directly into some logging system?
- What about when you want to change your method of logging?
	- Another [Log4Shell](https://en.wikipedia.org/wiki/Log4Shell) might come along and make your project vulnerable.
	- How can we minimise changing the code a bunch and have logging be more "plug & play"?

I'll be tackling all these points!

## Logging Interface

Let's take the question of "What about when you want to change your method of logging?". The obvious solution to this is
to have some sort of interface/bridge between our Java project code and the method of logging.

There are two on offer:

- [Simple Logging Facade for Java (aka SLF4J)](https://www.slf4j.org) is a simple
  logging [facade](https://en.wikipedia.org/wiki/Facade_pattern) for use in Java projects. It's built/maintained
  by [QOS.ch Sarl](https://qos.ch), a Swiss software company.

- [Commons Logging](https://commons.apache.org/proper/commons-logging) is a adapter for bridging your logging to another
  logging system. It's built/maintained by the [Apache Software Foundation](https://apache.org).

Which should you use? Simple answer: SLF4J.

Why?

1. SLF4J gives a [decent reason in their FAQ](https://www.slf4j.org/faq.html#why_new_project).
2. The SLF4J/QOS.ch developers expanded on
   the [issues with Commons Logging in 2009](https://articles.qos.ch/thinkAgain.html).
3. I've been doing development for 10 years now, and never seen Commons Logging in real usage that wasn't a legacy code
   base. So my experience/observations tell me that the modern solution is SLF4J.
4. As of writing, there
   are [518K references to the slf4j-api dependency](https://github.com/search?q=%3CartifactId%3Eslf4j-api%3C%2FartifactId%3E&type=code)
   on GitHub, and
   only [171K references to the commons-logging dependency](https://github.com/search?q=%3CartifactId%3Ecommons-logging%3C%2FartifactId%3E&type=code) -
   which, whilst it is a slightly flawed metric, at least implies that over 3x more projects are using SFL4J.

SLF4J code looks a bit like the following:

```java
package com.harmelodic;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.slf4j.MarkerFactory;

public class Application {
    private static final Logger logger = LoggerFactory.getLogger(Application.class);

    public static void main(String[] args) {
        logger.atInfo()
                .addKeyValue("args", args)
                .addMarker(MarkerFactory.getMarker("BOOT"))
                .log("Application started.");
        logger.info("A quick message");
        // ...
    }
}
```

## Logging implementation

Now that we have our interface to log stuff to (SLF4J), we need some implementation underneath that those logs and
actually log things!

Here are/were the options:

- [Log4j 1.x](https://logging.apache.org/log4j/1.x) is end of life. It was maintained by
  the [Apache Software Foundation](https://apache.org).

- [Log4j2](https://logging.apache.org/log4j/2.x) is the newer Log4j, built / maintained by
  the [Apache Software Foundation](https://apache.org). It is/was used widely... except it had
  the [Log4Shell vulnerability](https://en.wikipedia.org/wiki/Log4Shell) and now I wouldn't be surprised if no developer
  or organisation would touch it with a barge pole. Maybe it'll make a reprise... but I doubt it.

- [Logback](https://logback.qos.ch) is another "new Log4j", and has a lot of configurability to get logs just the way
  you want them. It's also maintained by the same [QOS.ch](https://qos.ch) folks who maintain SLF4J... which is a nice
  indicator. [Spring Boot currently defaults](https://docs.spring.io/spring-boot/reference/features/logging.html) to
  using Logback.

- The [Java Logging API](https://docs.oracle.com/en/java/javase/22/docs/api/java.logging/module-summary.html) (aka JUL)
  is a logging API built into Java, but... there's a reason why the logging libraries exist. JUL is a simple logging
  system, and if you want neat things like structured logging, filtering, or automatic log rotation, then you need a
  logging library.

For simple things, JUL can work. For everything else: Logback.

## Logback

This section is a very brief overview of the basic configuration of Logback. More information and advanced configuration
options can be read about in the [Logback manual](https://logback.qos.ch/manual/index.html).

View the Logback [artifacts on Maven Repository](https://mvnrepository.com/artifact/ch.qos.logback). I recommend
grabbing `logback-classic`, since that's the Logback dependency that hooks into SLF4J.

### Logback Configuration

Basic Logback configuration uses the following components:

- __Appenders__ - the things that say where the logs will be placed (Console, File, etc.)

- __Encoders__ & __Layouts__ - the things that take the Log Events from Logback, and prepares them for writing.
	- Encoders do that by transforming a Log Event into a byte array as well as writing out that byte array into an
	  `OutputStream`.
	- Layouts just transform the Log Event into a String.
	- Note: Often you can find yourself putting a Layout inside a `LayoutWrappingEncoder` Encoder - This is fine and
	  normal.

- __Patterns / Formatters__ are the configuration inside of Encoders and Layouts that informs that specific Encoder or
  Layout on how to format the log when it is written.

A typical simple Logback configuration file to output plain string logs to the console might look like:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- Pattern syntax: https://logback.qos.ch/manual/layouts.html#conversionWord -->
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg %kvp%n</pattern>
        </encoder>
    </appender>

    <root level="info">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```

### Structured Logging with Logback

When developing systems where logs are collected into a central system for querying & analysis, it is extremely useful
to implement "structured logging" - which in 2024 basically means that you output your logs into a consistent JSON
format.

To do this with Logback, you have a few choices (from my knowledge):

- The `logback-json-classic` and `logback-jackson` libraries are old "Logback Contrib" libraries available for
  Logback. "Logback Contrib" is the name for the old, official Logback open-source contributions community. The
  libraries are versioned to `0.1.5`, only ever had 5 versions, and was last updated in 2016, at time of writing.
  Probably not a good idea to use these, but they work, and are referenced a lot in guides for how to do JSON logging
  with Logback... so, clearly people like them.

- The [logstash-logback-encoder](https://github.com/logfellow/logstash-logback-encoder) library was initially built to
  put logs in the JSON format used by Elastic's [Logstash](https://www.elastic.co/guide/en/logstash/current/index.html)
  system, but is now a more generic structured logging library. It gets semi-regular updates, and works pretty well!

- Cloud Providers also can offer their own Logback implementations. For example,
  the [google-cloud-logging-logback](https://github.com/googleapis/java-logging-logback)
  and [spring-cloud-gcp-logging](https://github.com/GoogleCloudPlatform/spring-cloud-gcp/tree/main/spring-cloud-gcp-logging)
  libraries are built by Google and contain a bunch of code to support logging both directly and indirectly to the
  Google Cloud Logging API (previously known as Stackdriver).  
  Since it's all just implementations of Logback interfaces, you could use the `StackdriverJsonLayout` Layout with a
  regular `ConsoleAppender` to get structured JSON logs to your console.  
  Incidentally, Google's Logback library has the added benefit of being able to use and write your own
  `LoggingEventEnhancer`s, which allow you to add custom key-values into the JSON log output.

---

Thanks for reading!

~ Harmelodic ❤️
