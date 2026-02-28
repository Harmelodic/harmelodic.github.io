# Architecture Overview

Architecture can be a noun (_the_ architecture of a system) or a verb (engineers are "doing" architecture).

## What is Architecture (noun)?

> _It's the stuff that we wish we could get right at the start of a project._  
> ...  
> _It’s the shared understanding that the expert developers have of the system._
>
> ~ Ralph Johnson, Author of Design Patterns: Elements of Reusable Object-Oriented Software (1994)

This is pretty good definition of what architecture fundamentally is, although Dave Farley ended up making a slight
alteration to this definition that I think makes it better:

> _It's the snapshot of the shared understanding that the expert developers have of the system._  
> _We hope it doesn’t change too often, but we should expect it to change._
>
> ~ Dave Farley, Author of Modern Software Engineering (2021)

What I like about Dave Farley's definition, is what Dave Farley intended about his alteration: The fact that the
software architecture of a system is an evolving thing, and that the architecture at any one time is simply a snapshot.

## Why does Architecture (noun) help?

Architecture fundamentally gives us _structure_, and structure can a really useful thing. Good structure makes it easier
to tackle problems by:

- Making it easier to communicate with one another on “how things work” and what the vision is for a system.
- Making it easier to compartmentalise problems.
- Provides foundational principles, reasoning and design that we can rely on, to achieve greater reliability, greater
  speed of development, and better quality solution.

## How do we do Architecture?

In order to create architecture (noun), we need to do architecture (verb).

Doing architecture is effectively "making decisions" and ensuring that everyone is [aligned](../alignment.md) with those
decisions, in order for the executing software to match the designed architecture.

Architecture can be viewed from different perspectives. Ultimately, the practice of doing architecture is to collaborate
with others to construct a design for how a system will solve a problem.

There are different levels of depth that this can go into, which provides different kinds of value: _High-level_
architecture gives us an idea of the "big picture" and shows how different systems work together to achieve a larger
purpose. _System_ architecture is a lower-level architecture, which conveys how any one system is design to achieve its
goal (where the size of a system is subject to the size of the problem the system is intended to face).

> Some may argue that the _code_ itself can have architecture, which can be tested/enforced with tools like ArchUnit.
> Whilst some may call this architecture, I view this as simply "code structure" which is important in its own right,
> but I find limiting the term "architecture" to how systems are designed as a useful linguistic scoping to ensure the
> term is not diluted to the point where "architecture" is simply a synonym for "development".
>
> Similarly, "enterprise architecture" is also not really architecture, in my opinion, in that it deals much less with
> system design and more with organisational processes, broad technical strategy and, frankly, used as a fancy term for
> people to justify their position in an organisational hierarchy. Many tasks done by antiquated "enterprise architects"
> can instead be done by having technical leaders (senior engineers, principals, etc. ) collaborate with managers.

When thinking about these levels of architecture, I tend to immediately think
about [Domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design) (DDD) and "Domains". A domain is
essentially a business area. Complex domains can be broken up into subdomains, where each subdomain is effectively its
own domain, it just happens to be as part of a singular larger scope. Inside each domain, we have "Bounded Contexts"
which are the business models that model how the world works according to the system.

Applying this thinking to architecture, we can see that:

- High-level architecture is effectively identifying what domains (and subdomains) exist and how they might interact
  with one another (see more in [Domain Structure](#domain-structure)) and if there are non-functional requirements that
  are placed on any or all domains from a high-level perspective (
  see [Satisfying non-functional requirements](#satisfying-non-functional-requirements))
- System architecture is the design of the systems that implement the bounded contexts to fulfil the needs of domain (or
  subdomain).

To successfully architect systems that fulfil their purpose, we can use architectural design patterns and work closely
with domain experts.

The output of architecture often becomes a series of decisions made to improve the appropriateness of the design of a
system. In order to maintain alignment, promote transparency and aid inclusivity, it is extremely valuable to document
architectural decisions (as ADRs), and have a simple but well-defined set of requirements for making an architectural
decision. Read more below in [Making Architectural Decisions](#making-architectural-decisions).

## Who does architecture?

Historically, the software industry (especially in the "enterprise" space) had dedicated architects who tackled these
problems by drawing UML class models, sequence diagrams, etc. and communicating down to software developers who would
produce the code that the architects asked for. This was problematic as it often lead to (among other things):

- Fragile canonical models
- The software architect ending up in an "ivory tower" (where their "perfect" software design was too disconnected from
  the reality of the business domain or the needs of the system to be practical and/or useful).
- The software developers, architects and business folks all being quite disconnected on what is required, how the
  software should work, and what is actually possible.
- The software developers lacking in mandate from being able to make architectural decisions where they most expert
  knowledge (the implemented system).

Nowadays, the doing of software architecture is a little more decentralised. The software engineers who are responsible
for building systems handle the system architecture of those systems, and the high-level architecture is ideally a
collaborative alignment effort between all stakeholders, facilitated by experienced software engineers (though often
this is still handled by some central architect or architectural team).

## What influences architecture?

Purpose of the system creates functional requirements, which are fulfilled by the systems "features". These features
satisfied by systems working within a specific business model, which can influence architectural designs (e.g. systems
that offer functions that require storing historical, timestamped, event-based data will likely lead to an event-sourced
architecture being used).

Targeted user-base, system usage (current & predicted), system criticality, laws and more create non-functional
requirements, which influence a variety of architectural decisions that need to be made (and re-made).
See [below](#satisfying-non-functional-requirements) for more on this.

## Common architectures and patterns

Commonly though, having a small-user base tends to mean building a small, low-criticality system that will receive low
usage. These requirements lead to synchronous monolithic systems as these are quick to make and extend, as long as
complexity and requirements remain relatively low.

As requirements (functional and non-functional) and system complexity grows, we tend to "break things up" by turning
monoliths in services, or even microservices to increase scalability, resilience and performance (in some cases).
Systems that were once synchronously connected and tightly-coupled, need to be made loosely-coupled and often become
event-driven.

- Microservices vs Monoliths
- Synchronous vs Event-driven (vs Event-sourced)
- Patterns
	- Fetching data: Sync < Sync with Small cache < Sync with Large Cache < Sync with Offline API (nearly event-driven)
	- Inbox / Outbox
	- Saga / Distributed Transactions
	- Resilience Patterns (Bulkheads, Circuit Breakers, etc.)
	- More stuff at: [microservices.io/patterns](https://microservices.io/patterns/index.html)

### Satisfying non-functional requirements

- TODO
- List different kinds of non-functional requirements, and how to solve them with software patterns or decisions...
- Source list from knowledge and [this list](https://en.wikipedia.org/wiki/Non-functional_requirement#Examples).
- Other mechanisms that help solve problems: Pagination, Idempotency, etc.

### Domain structure

- Business domains
- Technical domains
- Entity domains
- Presentational domains
- Relationships between these domains
- Core/Supporting/Generic - what should you build and buy.

### Making Architectural Decisions

- Decisions, and the requirements for making a decision
	- What to we value in a decision?
	- It's a valid solution that solves an existing or upcoming problem.
	- It either aligns with previous decisions, or _explicitly_ changes architectural direction (which should prompt
	  re-evaluation of previous decisions to determine whether re-alignment is needed).
	- It was made transparently and inclusively.
- ADRs as documents
	- What do we value in this document?
	- It accurately reflects the decision.
	- It accurately documents the discussion and any possible alternatives that were considered in the decision.
	- It is timestamped.
- Example ADR and Decision-making process

### Naming things

Naming things is usually perceived as hard, but I don't think it is. If you're finding it hard, it's usually because:

- You don't know enough about the thing you're writing to know what a good name is.
- You're trying to make common data model, when it's not appropriate to do so.

When you know about what it is your writing, you critically think about what entities you're writing in your code, and
you build data models that fit the real world, then naming becomes a _natural_.

Here's some tips though:

- Name domains after their business domain.
- Name services after their business or system "role".
- Consult native-language speakers when working in an international work environment.
- If in doubt of a name/term, consult a domain expert.

## Software Design

- [Design Patterns for Humans](https://github.com/kamranahmedse/design-patterns-for-humans)
- [Domain-driven Design](https://en.wikipedia.org/wiki/Domain-driven_design)
- [Eight fallacies of distributed computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)
- [Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [GraphQL in 30 minutes](https://www.youtube.com/watch?v=UBGzsb2UkeY)
- [Interface control document](https://en.wikipedia.org/wiki/Interface_control_document)
- [Protocol Buffers Docs](https://protobuf.dev/)
- [Seven Layers of a System - OSI model](https://en.wikipedia.org/wiki/OSI_model)
- [Twelve-Factor App methodology](https://en.wikipedia.org/wiki/Twelve-Factor_App_methodology)
- [Microservices.io - Useful guidance when architecting with microservices](https://microservices.io/)
