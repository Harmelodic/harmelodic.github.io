# Build & Deploy: Maven projects

[Apache Maven](https://maven.apache.org), or just __Maven__, is a build tool for Java projects.

In this Build & Deploy post, I'll obviously not be talking about Deploy at all, but instead be covering the finer
details of generically building Java projects with Maven, including some recommendations for plugins and POM management.

## Building Maven projects

A normal Maven project is structured in the following way:

```
src/
  main/
  test/
  site/
target/
pom.xml
```

`src/main` if for your code. `src/test` is for your tests. `src/site` is for the website related to your project (can
probably ignore it). `target` is the output directory where artifacts are placed. `pom.xml` is the Maven configuration
file.

### The POM file structure

The [POM file](https://maven.apache.org/pom.html) is the main configuration file for your project - no need to have
several configuration files with their own config system, everything goes in the POM file.

The POM file has [a specific structure](https://maven.apache.org/pom.html#quick-overview), which features a lot of
different configuration options, but for the most part you'll probably just be using the following:

```xml

<project>
    <modelVersion>...</modelVersion>

    <parent>...</parent>

    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <packaging>...</packaging>

    <properties>...</properties>

    <dependencyManagement>...</dependencyManagement>

    <dependencies>...</dependencies>

    <build>...</build>
</project>
```

`modelVersion` is the Maven model version to use. As of 2024, you should use `4.0.0`.

`parent` is used to define if this POM inherits its configuration from another POM.

The `groupId`, `artifactId`, `version` elements are for basic project information, and should be self-explanatory (
check [the POM reference](https://maven.apache.org/pom.html) for more info).

`packaging` is another basic project element, which defines how we will be packaging this project into it's final form -
the default is `jar`. The setting of this element results in Maven configuring reasonably sensible defaults for the
build process for that packaging type.

`properties` are basically just variables to use in your POM. I recommend minimising the usage of these and use them
exclusively for inputs to your build process or setting properties that dependencies & plugins use. Setting dependency
versions in properties is a weird common practice that I've found makes maintenance more difficult.

`dependencyManagement` is for pulling in specific dependencies (other POMs) that will *manage* other dependencies for
you (i.e. which versions to use).

`dependencies` is the section where you define the dependencies (libraries, frameworks, etc.) you actually want to use
for your Java project. If these dependencies are managed by another POM (either `parent` or `dependencyManagement`
dependency) then you don't have to supply versions (helpful for maintenance).

`build` is to define what plugins to use in the build process.

### Maven Lifecycles

Maven has 3 built-in [lifecycles](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html) that
we work with, made up of phases.

The first lifecycle to know is the `default` lifecycle, which is as follows:

- `validate` - validate the project is correct and all necessary information is available
- `compile` - compile the source code of the project
- `test` - test the compiled source code using a suitable unit testing framework. These tests should not require the
  code be packaged or deployed
- `package` - take the compiled code and package it in its distributable format, such as a JAR.
- `verify` - run any checks on results of integration tests to ensure quality criteria are met
- `install` - install the package into the local repository, for use as a dependency in other projects locally
- `deploy` - done in the build environment, copies the final package to the remote repository for sharing with other
  developers and projects.

N.B. 1: Ignore the semantic meaning of `deploy` here, just treat it as a "publish" phase.

N.B. 2: There are more phases to the lifecycle, but it's likely you'll never need to use or interact with them.

A neat thing about the `default` Maven lifecycle, is that if you run a phase, say `mvn package`, it will run all the
phases up to and including the `package` phase. Better yet, when configuring Maven plugins, we can simply hook the
plugin into the lifecycle (if it doesn't hook itself).  
This makes creating a CI pipelines dead easy with Maven, since you never need to worry about running custom commands in
CI, instead you just run the lifecycle phase you want to run (plus some properties).

The second lifecycle is the `clean` lifecycle, which just usually empties the `target` directory in your Maven project (
where artifacts such as reports & JARs are placed).

The final lifecycle is the `site` lifecycle, which I've basically never used nor seen used in practice, so we'll ignore
that.

### Build Recommendations

This section pretty much exclusively focuses on the `build` section of the POM file, for the `default` lifecycle.

A build is made up of plugins. Each plugin has associated "goals". A goal is basically just some build code that will
run and do something. We can run goals either directly or bind them to a lifecycle phase.

The following plugins are very useful:

- `maven-pmd-plugin`: for validating code style/best-practices using [PMD](https://pmd.github.io).
- `maven-compiler-plugin`: for compiling code.
- `maven-surefire-plugin`: for running unit & some integration tests.
- `maven-failsafe-plugin`: for running some integration/integrated tests.
- `maven-jar-plugin`: for packaging into normal JARs.
- `maven-assembly-plugin`: for packaging into different types of artifacts, including JARs, which include their
  dependencies by extracting classes from dependency JARs (can cause class name conflicts)
- `maven-shade-plugin` for packaging into "Uber-JARs" by renaming dependency packages (preventing class name conflicts).
- `maven-install-plugin`: for installing JARs into your local Maven repository.
- `jib-maven-plugin`: for packaging into Container Images.
- `maven-deploy-plugin`: for publishing artifacts.
- `spring-boot-maven-plugin`: for packaging/running Spring Boot projects.

For different packaging types, Maven
will [bind specific plugins' & goals to the lifecycle phases](https://maven.apache.org/ref/3.9.8/maven-core/default-bindings.html)
by default. For example, for `jar` projects (the default), Maven will bind the following (and more):

- `maven-compiler-plugin`'s `compile` goal to the `compile` phase.
- `maven-surefire-plugin`'s `test` goal to the `test` phase.
- `maven-jar-plugin`'s `jar` goal to the `package` phase.
- `maven-install-plugin`'s `install` goal to the `install` phase.
- `maven-deploy-plugin`'s `deploy` goal to the `deploy phase.

For any Java project, I recommend configuring `maven-pmd-plugin`'s `check` goal to the `validate` phase.

For other different Java/Maven project types, other plugins can be used and bound to different phases. I'll recommend
those in more specific Build & Deploy blog posts.

### CI Recommendations

I recommend running `mvn clean verify` in Pull Requests, though depending on your project & testing strategy, you could
just run `mvn clean test` or `mvn clean package`.

I recommend running `mvn clean deploy` when wanting to publish, either on merge to `main` or after tagging with a
version (depending on your project and desired publishing process).

### POM Organisation Recommendations

Having everything is a single POM is fine for a couple of projects, but as you or your organisation starts building
multiple projects, it soon becomes painful to maintain individual POMs.

The recommended solution to this is to use `parent` POMs and `dependencyManagement` POMs (
aka [BOM POMs](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#bill-of-materials-bom-poms))
to offload most of the dependency management and build process. This leaves normal Maven projects to just define a
parent and BOMs to use and their dependencies.

I recommend organising this POM hierarchy as the following:

- A Root Parent POM which defines dependencyManagement, dependencies and build configuration for all projects.
- A Project Parent POM per project type (e.g. library, web-service, desktop-app, etc.) which inherits from the Root
  Parent POM.
- Use off-the-shelf BOMs, e.g. `junit-bom`, `spring-boot-dependencies`, `spring-cloud-gcp-dependencies`, and configure
  them in the relevant parent POM.
- Define your own BOM only if you have a LOT of internal libraries and want to co-ordinate them all together, and then
  configure it in the relevant parent POM (usually just the Root Parent POM).

Projects then just inherit from the relevant Project Parent POM, and everything should work out of the box.

---

Enjoy using Maven!

~ Harmelodic
