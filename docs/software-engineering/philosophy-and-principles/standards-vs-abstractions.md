# Standards vs Abstractions

Let's assume you help lead or manage a financially-stable organisation where you are producing and maintaining some kind
of product or service, and in doing so engage in a decent amount of software engineering.

You probably want to have all your engineers work more efficiently, and produce higher quality code. You could do a
variety of things such as investing in the education of engineers, or just hire better engineers, but there is something
you can do that provide a direct impact on the code produced and the sort of work done by the engineers:

Reduce technical complexity & unpredictability and increase ease of development by creating uniformity and
reproducibility in the code.

This means engineers need to think a little less about _how_ to implement systems and focus more on _what_ needs to be
implemented. Code becomes easier to produce and replicate. Interfaces / APIs become more homogenous and predictable
(even intuitive) and thus easier to integrate with. Moving between codebases or on different codebases also becomes
easier, as the code is similar in structure, style and implementation and thus feels familiar.

In practice, this can be achieved through two methods:

- Define **Standards** which are specifications, contracts and rules that can be enforced, primarily though mechanisms
  like Schema Validation and Code Analysis / Linting. Violations of the standards results in errors or warnings that
  prevent engineers from building non-standard systems.
- Create custom **Abstractions** which provide an interface for engineers to use in their code, and the implementation
  is taken care of elsewhere (e.g. in a library), where that implementation is subject to more scrutiny and maintained
  by experienced engineers.

So, the question becomes: which should we adopt? Maybe, both?

The immediate desire from a lot of engineers, managers, and product people is to create abstractions. Standards feel
like limitations whereas abstractions feels like simple tools. However, like with all things, there are trade-offs, and
you might be surprised to find that defining standards is actually much more powerful than it sounds, and abstractions
are more costly than they seem.

## The Trade-offs

TODO: Unpack this more.

- What trade-offs exist for each
	- Education / Learning / Knowledge trade-offs
	- Maintenance Cost trade-offs
	- Speed / Complexity / Implementation Cost trade-offs
	- Repetition trade-offs
		- Difference between actual toil and "work I don't like to do" -
		  reference the [Google SRE book on this](https://sre.google/sre-book/eliminating-toil/).
	- Blast-radius trade-offs
	- Culture trade-offs
	- Quality trade-offs
- What do we value
	- We value the _means_ of creation, not just _output_.
	- Short-term vs Long-term vs Iterative
	- Non-functional requirements / Architecture
		- Microservices > acceptance of code replication
		- Some systems are

TODO: Consider the following when writing the above:

- Tempting to build LEGO-brick abstractions, and engineers like the idea of building the next suite of composable
  systems, but the reality is quite different, especially in the long-term - especially as we see Fleet Management / PR
  Campaigns becoming easier with standardised codebases, scripts and tools like OpenRewrite.
- Devs directly uses industry-standard interfaces that they are familiar with, can learn more about and take skills with
  them when they leave.
- Devs use these interfaces in the simplest industry-standard way - We lint a bunch to ensure this - so we don't
  customise/over-engineer things we shouldn't and keep our lives simple (and easier to do fleet management / PR
  campaigns) and allow us to focus more time on the business requirements and operations.
- We try to use the default values & behaviour wherever possible to reduce the maintenance burden of being explicit
  everywhere and having to change a lot when upgrading on breaking changes for things we don't actually care about.
- We define extra standards where multiple industry standards exist to simplify our life, aid PR campaigns and increase
  the quality of our solutions.

## Analysis of trade-offs

As the above shows, abstractions really shine at having a very low implementation cost, at the cost of a higher
maintenance cost. Meaning in a situation where implementation cost is high, abstractions are especially useful. For
standards, they increase the quality of implementations, slightly reduce the implementation cost, but do not have as
high of a maintenance cost when compared with abstractions that achieve the same result.

As of 2025, in the age of cloud computing and a rich engineering ecosystem (lots of mature languages and tooling), the
implementation cost of building software is extremely low - granted some systems still have a high implementation cost.
However, having consistent _quality_ in those implementations is difficult to achieve without opinionated standards
(e.g. code analysis checks / linting rules).

Since for some cases, abstractions work better, and for others, standards work better; we can construct a methodology
that gives guidance on when to use abstractions and standards.

## The resulting methodology

For components or systems that are composable/reproducible and fit into one or more of the following:

- high complexity exists and doing it "right" only has 1 or 2 options,
- doing it wrong is very unsafe / too risky,
- they are deliberately custom to a specific component or use case or system architecture, which is unlikely to change,

then it is very useful to create libraries/abstractions to increase safety and hide unnecessary customisations and/or
complexity.

For everything else: it is more useful to standardise and provide reference implementations / samples of what "correct"
looks like, and build these standards on top of existing industry standards and open source/purchasable abstractions.  
I recommend creating these standards _with_ engineers, to create buy-in and a more inclusive and collaborative
engineering culture - and avoid "top-down decision-making" of these standards.

By doing this, we create the largest amount of engineering ease whilst minimising the biggest costs.

## What should you pick

Back to the analogy of "a financially-stable organisation where you are producing and maintaining some kind of product
or service, and in doing so engage in a decent amount of software engineering", you are probably in one of two kinds of
organisations:

- A Software Organisation: where your offering is software itself, that engineers or other organisations can use to make
  better software or their lives easier.
- A Product Organisation: where your offering uses software, or comes in the form of software, but in reality it's
  something else, like banking services, or a point-of-sale system, or a newspaper website, etc.

Before we go into each, I want to make it very clear that _**most**_ Organisations in the world that engage in software
engineering are NOT Software Organisations (even if they want to be) but are actually Product Organisations. For those
working in management, leadership, platform- or staff-engineering roles, it can be very tempting to compartmentalise and
silo oneself into thinking that "my department/team is effectively a Software Organisation inside a Product
Organisation" but that perspective has a major issue: In the big picture, you're still a Product Organisation and so
compartmentalising / siloing yourself is distracting and distancing yourselves from the core
purpose / [core domains](https://github.com/ddd-crew/core-domain-charts) of your Organisation.

### For Software Organisations

Congratulations, you're a pretty special case, and your literal offering is an **abstraction**. Any further abstractions
that are highly-related to your offering, or could also be reasonably offered and maintained are probably worthwhile
investments.

However, nearly everything _behind_ that abstraction should be simply **standardised** and abstractions avoided, with
the exception being things that are complex, unsafe or very deliberately custom.

### For Product Organisations

Nearly everything you build would benefit from being **standards** rather than **abstractions**.

Abstractions are useful for complex, unsafe or deliberately custom components, or if there is something directly offered
to users to improve your "unique selling point" (USP).

Creating custom abstractions / libraries to make things easier, or more reusable/less repeated, is a low priority, as
covered by the above [trade-offs](#the-trade-offs) and [resulting methodology](#the-resulting-methodology). Only when we
have a high capacity to tackle these things and other higher priority work is done can we create these libraries and
abstractions. Even then, we might not want to do it because we don't want to manage our own custom stuff, and so we
should contribute them to existing frameworks, libraries or at least create our own open source stuff, so that they can
BE industry-standard components - at which point the Product Organisation is becoming a Software Organisation (at least
partly).

## Real-world examples

Here are some real world examples of the above [philosophy](#the-resulting-methodology) put into practice:

- Good Abstractions:
	- Complex networking configuration for Ingress/Egress
	- Authorization implementation(s) that cross-cut across all software, and creates a critical problems if implemented
	  incorrectly.
	- Critical systems that all share the architecture, but need to be separate systems for resiliency and semantics.
	- Simple centralised application configuration (that all applications inherit or inject) that handles the
	  core bootstrapping of the application into a platform (e.g. observability, health and other fundamental components
	  required for an application to even _be_ an application) and enforces standards (e.g. configures rules, linters,
	  and default values that enforce internal standards).
- Failed Abstractions:
	- Code generation of code to simplify an abstraction into a simpler abstraction. Failed because the underlying
	  abstraction complexities and configurability ended up being needed, and the maintenance burden of the simplified
	  abstraction was not worth it because we couldn't hope to achieve the same level of support, documentation,
	  features that the underlying abstraction offered (especially since it was open-source).
	- Custom client libraries that configured clients "the right way" and provided a simplified interface. Ultimately
	  proved useless as existing clients were relatively easy to build, standardisation/linting rules ensured
	  correctness, and frameworks / libraries improved their interfaces to make it even easier to implement clients -
	  also actively impaired the knowledge, learning and critical thinking of developers as it hid vital knowledge
	  behind a custom, non-industry standard or open source abstraction.
- Good Standards:
	- API standards to improve quality, improve uniformity of API behaviour, and further satisfy the principle of least
	  astonishment.
	- Specific languages and/or technologies being used for specific purposes to reduce choice / technological drift.
	- Specific technology configurations to make fleet management easier and software engineering simpler to jump into.
	- Limiting infrastructure to only be created as code (never manually) to improve resiliency & quality in
- Failed Standards:
	- Single language used for all purposes. Failed as there is no one programming language that fits all purposes, so
	  the technical limitations enforced by the standard hindered the ability to produce fit-for-purpose solutions. Also
	  frustrated a significant portion of engineers and made for an unpleasant work environment.
