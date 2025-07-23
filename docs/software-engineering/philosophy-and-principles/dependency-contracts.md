# Dependency Contracts

Dependencies (like libraries, BOMs, etc.) tend to be version controlled - often
using [Semantic Versioning](https://semver.org/). When updating or changing a dependency, we will also change the
version to reflect the change. Version systems like Semantic Versioning offer a difference between a _breaking_ change
and a non-breaking change - where a breaking change is where you make API changes that are incompatible with the
previous version.

The API is effectively the _contract_ between the dependency and the consumer of the dependency. This contract usually
covers things like the classes, functions, methods, etc. that the dependency offers, but different dependencies offer
different functionality - and sometimes the behaviour / implementation details of the dependency is part of the API
contract, or becomes part of an unspoken API contract over time as consumers depend more on the behaviour of the
dependency and not the API - this becomes a point of conflict if the API / contract is not well-defined.

This is why I tend to advocate for dependency maintainers to write an explicit contract for what the dependency offers
and does not offer. This means that conversations about what kind of version bump (major, minor or patch) should be used
and also makes clear to consumers of the dependency what they can and should be dependent on.

I recommend putting this contract with the `README.md` of the repository that contains the dependency.

Below are a few example contracts of different kinds of dependencies. They use the Java / Maven ecosystem, as that is
what I am most familiar and comfortable with, but they could be adapted to any dependency for any ecosystem.

## Parent for an Application

TODO

## Bill of Materials

TODO

## Library

TODO

## Application Platform Starter Library

An application platform starter library is a library that will pull in dependencies and configure the core components
that is needed to bootstrap an application into running correctly on a specific platform (things like observability
configuration and custom core platform configuration).

TODO
