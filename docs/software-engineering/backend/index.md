# Backend Overview

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

With:

- Specific principles, architectural practices
- Operational work (Ops)
- Development lifecycle
- Language/Stack specifics (per language/stack)
	- Local machine setup
	- CI (validate, test, build)
	- CD (deploy, release)
	- Doing specific things in that language
