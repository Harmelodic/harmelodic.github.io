# Backend Overview

If the User Interface (UI) is the "Frontend" then, the stuff that happens behind the scenes is the Backend.

This effectively means:

- Providing an API to a Frontend (or for API access).
- Performing business logic.
- Storing the data appropriately.
- Communicating with other systems.

In web services (the usual backend), this translates to:

- APIs as HTTP-based APIs (HTTP, REST, SOAP).
- Business logic in "services" which are in modern engineering often "microservices".
- Storing and accessing data in a variety of ways including:
	- SQL databases
	- Document databases
	- Key-Value stores
	- Blob storage
	- File storage
	- Message/Event Buses (often temporarily (< 30 days))
- Communicating to other systems via APIs and Message/Event buses.

As systems tackle greater complexity, lots of other software engineering techniques come into play such as:

- Understanding and meeting business needs.
- Building libraries to share & reuse code between systems / engineers.
- Scheduling to perform scheduled processes or "jobs".
- Batching to increase throughput and efficiency.
- Different models of Authentication & Authorization (both for Users and for Systems).
- Integrating with 3rd party services for specialised non-core domain processing and tasks.
- Producing data for data warehouses or lakes (see [Data Science](../data-science/index.md))
- Installation, management and integration of open-source software for generic but specific tasks (e.g. Search,
  Messaging or Authorization).
- Alerting, Monitoring, Optimising and Troubleshooting using various Observability / Telemetry tools (also
  see [Platform Engineering](../platform/index.md))
- Operating and maintaining running services (scaling, resourcing, configuring, etc.)
- Releasing and rolling back new versions of services, as appropriate.

... and probably more.

## TODO

- Java
	- Setup Java on a machine (sdkman, maven, maven wagons?)
	- Maven setup (Single repo per artifact, BOMs, parents, libraries, GAV naming)
	- Package structure
	- Spring recommendations (Dependency Injection, Controllers, Services, JDBC/ORMs, HTTP Clients)
	- Testing (JUnit5, Jupiter Assertions, WireMock / PACT, Testcontainers, @SpringBootTest)
	- Observability (Logging, Metrics, Tracing)
	- Code style specifics (Never `var`, Use streams, Imperative OOP not reactive)
- Go
- Rust?
- Databases / Storage (SQL/Relational, In-memory key-value, Document, Blob / File Storage)
- Messaging (Queues, PubSub)

## Java

- [Adoptium Temurin JDK](https://adoptium.net/en-GB/temurin/releases/)
- [Apache POI - Processing Microsoft Office Docs](https://poi.apache.org/)
- [Arquillian - Integration Testing Framework](https://docs.jboss.org/arquillian/reference/1.0.0.Alpha1/en-US/html_single/)
- [Gradle](https://gradle.org/)
- [JUnit](https://junit.org/)
- [Java 20 - Javadoc](https://docs.oracle.com/en/java/javase/20/docs/api/index.html)
- [JavaFX Architecture](https://docs.oracle.com/javase/8/javafx/get-started-tutorial/jfx-architecture.htm)
- [Learn Java - dev.java](https://dev.java/learn/)
- [Maven](https://maven.apache.org/)
- [Maven Repository - Raw](https://repo.maven.apache.org/maven2/)
- [Multithreading in Java](https://www.tutorialspoint.com/java/java_multithreading.htm)
- [OpenJDK JEPs](https://openjdk.org/jeps)
- [Phil's Data Structure Zoo](https://g1thubhub.github.io/data-structure-zoo.html)
- [Practical Money in Java](https://jdriven.com/blog/2020/05/Practical-Money-InJava)
- [SDKMAN!](https://sdkman.io/)
- [Spring - Framework](https://spring.io/)
- [javadoc.io - Free Javadoc Hosting](https://javadoc.io/)

## Go

- [Go Styleguide](https://google.github.io/styleguide/go/)
- [Standard Library - Go Packages](https://pkg.go.dev/std)

## Rust

- [Cargo - Reference](https://doc.rust-lang.org/cargo/reference/index.html)
- [Rust Playground](https://play.rust-lang.org/)
- [The Rust Book](https://doc.rust-lang.org/book/title-page.html)
- [crates.io](https://crates.io/)
- [gtk - crate](https://crates.io/crates/gtk)
- [imgui - crate](https://crates.io/crates/imgui)
- [qt_core - crate](https://crates.io/crates/qt_core)

## Node.js

- [Electron - Desktop Framework](https://www.electronjs.org/)
- [Express - Framework](https://expressjs.com/)
- [The Event Loop - Philip Roberts' JSConf Talk](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [Useful Node.js Modules](https://www.codementor.io/@ashish1dev/list-of-useful-nodejs-modules-du107mcv3)

## Databases

- [H2 - in-memory for Java](https://www.h2database.com/html/main.html)
- [MongoDB](https://docs.mongodb.com/)
- [MySQL](https://dev.mysql.com/)
- [PostgreSQL](https://www.postgresql.org/docs/)
- [SQL Server by Microsoft](https://learn.microsoft.com/en-gb/sql/sql-server)
- [SQLite](https://www.sqlite.org/)
- [Pool Sizing advice - HikariCP](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing)

## Data Processing

- [Apache Camel](https://camel.apache.org/)
- [Apache Kafka](https://kafka.apache.org/)
- [Apache NiFi - User Guide](https://nifi.apache.org/docs/nifi-docs/html/user-guide.html)
- [Apache Tika - Metadata Extraction](https://tika.apache.org/)
- [Cloudera - Managed Apache Software](https://www.cloudera.com/)
- [GCP Dataflow](https://cloud.google.com/dataflow)
- [GCP Pub/Sub](https://cloud.google.com/pubsub)
- [Spring Integration](https://spring.io/projects/spring-integration)

## Handling Payments

- [PayPal Braintree](https://developer.paypal.com/braintree/docs/)
- [Square API](https://developer.squareup.com/reference/square)
- [Stax Payments API](https://docs.staxpayments.com/)
- [String Payments API](https://stripe.com/docs/payments)
- [Trustly Payments API](https://eu.developers.trustly.com/doc/reference/)

## Performance Testing

- [Artillery](https://www.artillery.io/)
- [JMeter](https://jmeter.apache.org/)
- [k6](https://k6.io/)
- [Locust](https://locust.io/)j
- [Testkube](https://testkube.io/)
- [UL Benchmarks](https://benchmarks.ul.com/)

## Handling Email

- [Apache James](https://james.apache.org/)
- [Mailchimp](https://mailchimp.com/)
- [Postfix](https://www.postfix.org/)
- [roundcube](https://roundcube.net/)
- [Sendgrid](https://sendgrid.com/)
