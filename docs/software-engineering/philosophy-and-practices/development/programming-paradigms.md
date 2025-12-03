# Programming Paradigms

TODO: Brief overview of the following:

- Imperative
	- Procedural
	- Object-oriented
- Declarative
	- Functional
	- Reactive
	- Logic
- Scripting, why I'm treating it as a separate paradigm.
	- Event-based
	- Scripts react to things happen in a separate but linked system (e.g. JavaScript -> HTML DOM / Browser)
- Aspect-oriented programming (AOP) / eBPF code-injection & instrumentation
- A note on Concurrency and how it fits in to other paradigms but has its own complexity.

TODO: Make the point that:

- No single paradigm is fully "better" than the others. Instead, each have aspects of them that make different paradigms
  useful for particular purposes.
- For some purposes this makes the decision to use them for specific purposes abundantly clear.
- For other purposes, the trade-offs are too close to call or not very clearly worth it, and so I've found its generally
  better [standardise](./standards-vs-abstractions.md) on a programming language / paradigm that will be good for that
  ecosystem (e.g. backend) since you'll broadly gain more benefits to the code and organisation from the standardisation
  than you will get code/performance benefits from picking different languages for different problems.
- Examples of different paradigms for different purposes:
	- Config = Declarative (go into linting, formatting and validation over testing)
	- Backend business logic/processing = Imperative &/or OO
	- Mass Data processing = Declarative pipelines plus declarative or imperative workflows.
	- Network-connected client = Scripting + hand-off to backend or data systems.
	- Offline client = Imperative &/or OO.

Some may argue that event-based architectures work nicer with declarative / functional programming languages, and whilst
I often agree with their arguments and thoughts, the practicality / reality of working with these languages is that
event-based architectures are still easily created with imperative languages, and it's easier to do engineering &
troubleshooting (and make less mistakes) with imperative instructions than it is to with declarative abstractions - and
then for certain tasks we can adopt a declarative style where it's appropriate and easier to implement (e.g. Java 8
streams).
