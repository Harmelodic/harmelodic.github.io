# Java, the JDK, and the JRE

To compile Java, you need a Java Development Kit (a JDK). To run Java, you need a Java Runtime Environment (JRE).

## JDKs

A JDK is basically an implementation of the Java Standard, with a bunch of associated tools. The official reference
implementation is OpenJDK, but there are other implementations / variants that offer extra security or extra tools or
support / help:

- Eclipse Temurin / Adoptium (previously called AdoptOpenJDK)
- Oracle
- Amazon Corretto
- and a bunch of others

I tend to use Temurin, since it has no licensing fees, has the standard JDK tools I expect, is well-supported by the
open source community and the Eclipse Foundation, and is pretty performant / secure.

## JREs

JDKs include a JRE, so for development and building Java projects, you don't need an extra component in order to compile
and execute Java (e.g. for tests).

However, when running compiled Java byte code (as a compiled artifact (e.g. a JAR)), you only need the JRE and any
dependencies (assuming they're not built into the JAR, as a "fat" JAR or "uber" JAR).

In fact, shipping only a JRE (and not the full JDK) is a good security practice, since you're not shipping a JDK (with
all the extra tools) to a production environment, which could be subsequently used by hackers or malware if they somehow
gained access to the host where your JRE is running.

## Java needs no .java-version file

Many languages like Python, Terraform, Node and Go can benefit from a `.<lang>-version` file checked into version
control. This means that you can grab the code, install the version of the language specified in the language file, and
you're ready to go! Not only is this convenient, but it solves a problem for us: Different versions of the language
support (or don't support) specific language features - in order to guarantee compilation / execution, we need to use a
specific version of the language runtime / compilation tools.

For some reason, this seems to have spread to the Java world with a `.java-version` file appearing. However, this is not
really needed, because any JDK can compile Java code to any target version of Java of the same version or below - so
there is no need to use a specific JDK, as the latest will do!

[JDK-tied libraries and tools](#jdk-tied-libraries-and-tools) can be the exception to this, if they are not up to date,
though even these have workarounds in order to be able to use the latest JDK soon after it is released.

## Java components

For compiling Java, there are 3 main components to reckon with:

- Java Development Kit (JDK)
- Java byte code
- Java Runtime Environment (JRE)

The JDK provides compilation tools for compiling Java code to Java byte code. Java byte code runs on a JRE.

The JDK and JRE are backwards compatible. This means that:

- A JDK of a version can compile to Java byte code of any version less than or equal to the JDK version (e.g. JDK 21 can
  compile to Java byte code for Java 21 or 20 or 19, and so on).
- A JRE of a version can run Java byte code of any version less than or equal to the JRE version (e.g. JRE 21 can run
  Java byte code of version Java 21 or 20 or 19, and so on).

This means that we can always use the latest JDK to compile our code, but target to different versions of Java as
needed (compile for older versions for compatibility, or newer for new features).

## JDK-tied libraries and tools

Some libraries / tools interact with the JDK itself to provide features. These libraries / tools need to support new JDK
versions in order to use the latest JDK with these tools.

If you use one of these tools in your project, you can be hindered in upgrading to using the latest JDK, at least until
support is released (which for some libraries / tools can be a few months or longer!):

- Project Lombok
- The Modernizer Maven Plugin
- PMD
- Spotbugs
- Any other tool that uses the ASM framework

Often tools just need to bump to a newer version of ASM in order to support the latest JDK, so overriding the ASM
version by directly depending on it can be a useful, if hacky, way to maintain using the latest JDK.

In the case of Lombok, I recommend just not using it, as JDK support is often delayed and overriding the ASM framework
version does not always work as a workaround.
