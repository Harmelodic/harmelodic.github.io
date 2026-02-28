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
	- Authentication = Proving identity.
	- Authorization = Proving allowed access.
	- Session-based vs token-based vs mixed/combination.
	- Terminology and what to use (Login / Logout / Sign-in / Register, IAM, 2FA, MFA).
	- Common tools (Keycloak, OpenFGA, Auth0, OIDC, SSO, Oauth2.0 (apps & 3rd party login), SAML, JWT, JWKS, Cloud IAM).
	- Auth methods/factors:
		- Something you know (e.g. password),
		- Something you have (e.g. id card, passport).
		- Something you are (e.g. biometrics (fingerprint, face, eye)).
- Integrating with 3rd party services for specialised non-core domain processing and tasks.
- Producing data for data warehouses or lakes (see [Data Science](../data/index.md))
- Installation, management and integration of open source software for generic but specific tasks (e.g. Search,
  Messaging or Authorization).
- Alerting, Monitoring, Optimising and Troubleshooting using various Observability / Telemetry tools (also
  see [Platform Engineering](../platform/index.md))
	- Backend focus on implementing these things for services, and what to alert/monitor/optimise/troubleshoot
	- Actionable alerts
	- Intuitive monitoring
	- Optimise where necessary, otherwise reduce complexity
	- How to
	  use [the 4 parts to Metrics & Telemetry](https://grafana.com/docs/grafana/latest/explore/simplified-exploration/metrics/about-metrics/)
	  (also covered in Platform from a Platform Engineering perspective).
	- Common SLIs and SLOs for Backend
	- [RED and USE](https://pagertree.com/learn/devops/what-is-observability/use-and-red-method)
- Operating and maintaining running services (scaling, resourcing, configuring, etc.)
- Releasing and rolling back new versions of services, as appropriate.

... and probably more.

## TODO

- Java / JVM
	- Setup Java on a machine (sdkman, maven, maven wagons?)
	- General project management (Single repo per artifact vs multi-module, BOMs, parents, libraries, GAV naming)
	- Maven vs Gradle vs others
	- Package structure
	- Spring recommendations (Dependency Injection, Controllers, Services, JDBC/ORMs, HTTP Clients)
	- Testing (JUnit5, Jupiter Assertions, WireMock / PACT, Testcontainers, @SpringBootTest)
	- Observability (Logging, Metrics, Tracing)
	- Code style specifics (Never `var`, Use streams, Imperative OOP not reactive)
	- Java vs Kotlin (also Scala and Groovy)
- Go
	- Same as above, but how?
	- [Effective Go](https://go.dev/doc/effective_go)
- Rust?
- Databases / Storage (SQL/Relational, In-memory key-value, Document, Blob / File Storage)
- Messaging (Queues, PubSub)
- AI agent systems (ew, hate this, but also can't ignore it, and if it gets less yucky, it'd be useful to have a point
  of reference)
	- Model Context Protocol (MCP)
	- For more on building AI models, see [Data Science](../data/index.md)

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

## Data Processing / Messaging

- [Apache Camel](https://camel.apache.org/)
- [Apache Kafka](https://kafka.apache.org/)
- [Apache NiFi - User Guide](https://nifi.apache.org/docs/nifi-docs/html/user-guide.html)
- [Apache Tika - Metadata Extraction](https://tika.apache.org/)
- [AWS Kinesis (Stream processing)](https://docs.aws.amazon.com/kinesis/)
- [AWS SQS (Queue / Messaging)](https://docs.aws.amazon.com/sqs/)
- [Cloudera - Managed Apache Software](https://www.cloudera.com/)
- [GCP Dataflow](https://cloud.google.com/dataflow)
- [GCP Pub/Sub](https://cloud.google.com/pubsub)
- [NATS](https://nats.io/)
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

## Handling Multimedia

- [Gstreamer](https://gstreamer.freedesktop.org) - multimedia framework for audio & video processing.
- [FFmpeg](https://ffmpeg.org) - record, convert and stream audio and video.
- [Project Janus](https://www.microsoft.com/en-us/research/project/programmable-ran-platform/) - Radio Access Networks
  (RAN) programming & tooling.
- [Real-time Transport Protocol (RTP)](https://en.wikipedia.org/wiki/Real-time_Transport_Protocol) - transporting audio
  & video over IP - built on UDP.
- [Audio-over-IP (AoIP)](https://en.wikipedia.org/wiki/Audio_over_IP) / Audio contribution over IP (ACIP).
- [Audio Engineering Society (AES)](https://en.wikipedia.org/wiki/Audio_Engineering_Society) - organisation &
  standardisation body for audio engineering.
- [MADI / AES10](https://en.wikipedia.org/wiki/MADI) - _Multichannel Audio Digital Interface_ standard.
- [AES67](https://en.wikipedia.org/wiki/AES67) - standard for AoIP.
- [Dante](https://en.wikipedia.org/wiki/Dante_(networking)) - software, hardware & protocols for transporting
  professional audio.
- [Ravenna](https://en.wikipedia.org/wiki/Ravenna_%28networking%29) - software for real-time transport of audio over IP.
- [Formula (VST)](https://github.com/soundspear/formula) - Programmable audio effect plugin (program using C)

## API Specifications

- [AsyncAPI](https://asyncapi.io/)
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Spectral API Style Guide & Linting](https://stoplight.io/open-source/spectral)
