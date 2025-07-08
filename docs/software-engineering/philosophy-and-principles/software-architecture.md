# Software Architecture

## What is Architecture?

> _It's the stuff that we wish we could get right at the start of a project._
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

## Why does Architecture help?

Architecture fundamentally gives us _structure_, and structure can a really useful thing. Good structure makes it easier
to tackle problems by:

- Making it easier to communicate with one another on “how things work” and what the vision is for a system.
- Making it easier to compartmentalise problems.
- Provides foundational principles, reasoning and design that we can rely on, to achieve greater reliability, greater
  speed of development, and better quality solution.

## How do we do Architecture?

Architecture can be viewed from different perspectives. Ultimately, the practice of doing architecture is to collaborate
with others to construct a design for how a system will solve a problem.

There are different levels of depth that this can go into, which provides different kinds of value. _High-level_
architecture gives us an idea of the "big picture" and shows how different systems work together to achieve a larger
purpose. _System_ architecture is a lower-level architecture, which conveys how any one system is design to achieve its
goal (where the size of a system is subject to the size of the problem the system is intended to face).

When thinking about these levels of architecture, I tend to immediately think
about [Domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design) (DDD) and "Domains". A domain is
essentially a business area. Complex domains can be broken up into subdomains, where each subdomain is effectively its
own domain, it just happens to be as part of a singular larger scope. Inside each domain, we have "Bounded Contexts"
which are the business models that model how the world works according to the system.

Applying this thinking to architecture, we can see that high-level architecture is effectively identifying what
domains (and subdomains) exist and how they might interact with one another, and system architecture is the design of
the systems that implement the bounded contexts to fulfil the needs of domain.

So, in order to do architecture, we must: understand the real world by collaborating with business experts, model the
real world in order to achieve 