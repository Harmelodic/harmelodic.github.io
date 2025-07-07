# Some Software Development Principles

> Originally published: 01 May 2020

Developing software can be a complicated business. Thankfully, various principles are on hand to help engineers make
good design decisions.

Here are some that have helped me.

## KISS

Keep it simple, stupid  
or  
Keep it small & simple <sub>[[C.H.]](https://twitter.com/CliveHackney/status/1257013748725940224)</sub>

## Test Pyramid

<img src="https://harmelodic.gitlab.io/scribbles-content/posts/images/test-pyramid.png" alt="Test Pyramid" style="width: 90%; max-width: 560px;">

- Unit Tests are fast and cheap, so make more of your tests Unit Tests.
- Service Tests are slower and more expensive, so only make some of those.
- UI tests are very slow and super expensive, make as few as possible.

## 3 Types of Functional Testing - State, Collaboration, Contract

<img src="https://harmelodic.gitlab.io/scribbles-content/posts/images/functional-tests.jpg" alt="Functional Tests" style="width: 90%; max-width: 560px;">

Provided you design your solution well - i.e. into components that connect to interfaces that have other components that
implement those interfaces.

- State testing is the testing you do on the component you've made.
- Collaboration testing is the testing you do between your component and it's interfaces.
- Contract testing is the testing you do to check your external interface implementations adhere to the interface in the
  way that your component expects them to (aka - adhering to the Contract).
- The only other _functional_ testing that is required is the few integrated tests that you need to test components
  outside of your domain.

This is my own principle based on J.B. Rainsburger's talk about how [_Integrated Tests are a
Scam_](https://blog.thecodewhisperer.com/permalink/integrated-tests-are-a-scam). It's a very good talk with very
compelling points.

Non-functional testing, such as security, performance and reliability and acceptance testing cannot be fully covered by
this methodology.

## Testing Permutations

When testing a function that return Collections, ensure you have a test for each of the following:

| Scenario | Explanation                                                                                          |
|----------|------------------------------------------------------------------------------------------------------|
| Zero     | When the function returns nothing                                                                    |
| One      | When the function returns a single entity                                                            |
| Many     | When the function returns a list of entities                                                         |
| Lots     | When the function returns an absurd amount of entities (for: pagination, filtering, long-load times) |
| Oops     | When the function errors                                                                             |

## 12 Factor App

| Factor              | Description                                                                                                         |
|---------------------|---------------------------------------------------------------------------------------------------------------------|
| Codebase            | There should be exactly one codebase for a deployed service with the codebase being used for many deployments.      |
| Dependencies        | All dependencies should be declared, with no implicit reliance on system tools or libraries.                        |
| Config              | Configuration that varies between deployments should be stored in the environment.                                  |
| Backing services    | All backing services are treated as attached resources and attached and detached by the execution environment.      |
| Build, release, run | The delivery pipeline should strictly consist of build, release, run.                                               |
| Processes           | Applications should be deployed as one or more stateless processes with persisted data stored on a backing service. |
| Port binding        | Self-contained services should make themselves available to other services by specified ports.                      |
| Concurrency         | Concurrency is advocated by scaling individual processes.                                                           |
| Disposability       | Fast startup and shutdown are advocated for a more robust and resilient system.                                     |
| Dev/Prod parity     | All environments should be as similar as possible.                                                                  |
| Logs                | Applications should produce logs as event streams and leave the execution environment to aggregate.                 |
| Admin Processes     | Any needed admin tasks should be kept in source control and packaged with the application.                          |

## Unix Philosophy

- Write programs that do one thing and do it well.
- Write programs to work together.
- Write programs to handle text streams, because that is a universal interface.

## Domain-driven Design

Goals:

- Placing the project's primary focus on the core domain and domain logic.
- Basing complex designs on a model of the domain.
- Initiating a creative collaboration between technical and domain experts to iteratively refine a conceptual model that
  addresses particular domain problems.

Where the model concepts include:

| Concept             | Description                                                                                                                                   |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| Context             | The setting in which a word or statement appears that determines its meaning.                                                                 |
| Domain              | A sphere of knowledge (ontology), influence, or activity. The subject area to which the user applies a program is the domain of the software. |
| Model               | A system of abstractions that describes selected aspects of a domain and can be used to solve problems related to that domain.                |
| Ubiquitous Language | A language structured around the domain model and used by all team members to connect all the activities of the team with the software.       |

## Test-driven development

1. Write your test
2. Prove it fails
3. Write your code
4. Prove it works
5. Refactor your solution as necessary
6. Prove it still works

## SOLID

| Principle             | Description                                                                                                                                                                  |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Single-responsibility | A class should only have a single responsibility, that is, only changes to one part of the software's specification should be able to affect the specification of the class. |
| Open–closed           | Software entities ... should be open for extension, but closed for modification.                                                                                             |
| Liskov substitution   | Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program." See also design by contract.                  |
| Interface segregation | A language structured around the domain model and used by all team members to connect all the activities of the team with the software.                                      |
| Dependency inversion  | One should depend upon abstractions, not concretions.                                                                                                                        |

## CAP Theorem

It is impossible for a distributed data-store to simultaneously provide more than _two_ out of the following three
guarantees:

| Guarantee           | Description                                                                                                                    |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|
| Consistency         | Every read receives the most recent write or an error.                                                                         |
| Availability        | Every request receives a (non-error) response, without the guarantee that it contains the most recent write.                   |
| Partition tolerance | A system of abstractions that describes selected aspects of a domain and can be used to solve problems related to that domain. |

## ACID

When developing database transactions, abiding by the following principles guarantee validity even in the event of
errors:

| Property    | Description                                                                                                                                            |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| Atomicity   | The transaction must either completely succeed or completely fail. If succeeded, the entire transaction occurs. If failed, the database is unchanged.  |
| Consistency | The transaction must bring the database from one valid state to another valid state.                                                                   |
| Isolation   | If transactions are executed concurrently, the database MUST achieve the same state that it would have if the transactions were executed sequentially. |
| Durability  | Once a transaction is committed, it will remain committed even in the case of system failure.                                                          |

## Agile

- **Individuals and interactions** over processes and tools
- **Working software** over comprehensive documentation
- **Customer collaboration** over contract negotiation
- **Responding to change** over following a plan

That is, while there is value in the items on the right, we value the items on the left _more_.
