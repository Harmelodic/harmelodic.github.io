# Standards vs Abstractions

> TL;DR:
>
> In modern software engineering, in order to improve software quality and ease of software engineering, Standards (like
> specifications, schemas, linting, and other code rules) are **better** than Abstractions (like libraries, modules and
> APIs).
>
> However, abstractions are still useful for dealing with reproducible code that is complex, critical/unsafe,
> deliberately custom.

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
easier, as the code is similar in structure, style and implementation and thus feels familiar. Building software feels
easy and fun, akin to putting LEGO bricks together.

In practice, this can be achieved through two methods:

- Define **Standards** which are specifications, contracts and rules that can be enforced, primarily though mechanisms
  like Schema Validation and Code Analysis / Linting. Violations of the standards results in errors or warnings that
  prevent engineers from building non-standard systems. Fixes to wide-spread issues can be resolved through automated
  code refactoring.
- Create custom **Abstractions** which provide an interface for engineers to use in their code, and the implementation
  is taken care of elsewhere (e.g. in a library), where that implementation is subject to more scrutiny and maintained
  by experienced engineers. Fixes to wide-spread issues is implemented centrally and then received through a version
  bump.

So, the question becomes: which should we adopt? Maybe, both?

The immediate desire from a lot of engineers, managers, and product people is to create abstractions. Standards feel
like limitations whereas abstractions feels like simple tools. However, like with all things, there are trade-offs, and
you might be surprised to find that defining standards is actually much more powerful than it sounds, and abstractions
are more costly than they seem.

## The Trade-offs

Trade-offs are based on what we value. As covered in [Values](../values.md), we care about aspects of both the Software
and
Software Engineering. Standards vs Abstractions ultimate goal is to make _Software_ cheaper and _Software Engineering_
easier, but most of other trade-offs are ones that touch upon Software Engineering values (some of which are often
highly valued by software engineers but under-considered by managers).

The following table shows different aspects where trade-offs exist between Standards and Abstractions approaches, and to
what degree the approach that aspect:

| Aspect                                                              | Standards                                                                                                          | Abstractions                                                                                                                                  |
|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| Change in Implementation cost / complexity for end-user engineers   | Moderately decreased, as standards enforce/guide implementation design.                                            | Largely decreased, as implementation is handled centrally.                                                                                    |
| Change in Maintenance cost / complexity for end-user engineers      | Moderately decreased, as code design becomes uniform across codebase.                                              | Largely decreased, as maintenance just becomes bumping versions and fixing interface changes.                                                 |
| Implementation cost / complexity of approach                        | Slightly costly, as rules / standards need to be defined and applied, but it's only rules.                         | Moderately or Largely costly, as implementation code is centralised, and could still be complex.                                              |
| Maintenance cost / complexity of approach                           | Slightly costly, as rules / standards need to be updated to match modern software practices.                       | Moderately costly, as implementation needs to be update to match modern software practices and fulfil new implementation requirements.        |
| Aids engineering learning / knowledge                               | Actively spreads knowledge and informs engineers what good quality. Engineers use industry-standard abstractions.  | Actively hides knowledge and good quality behind an abstraction. Engineers use and learn the org-specific abstractions and "way".             |
| Increases quality of code                                           | Increases implementation quality by enforcing good practices and discouraging over-engineering.                    | Can increase implementation quality (if implementation actually follows good practices), though over-abstraction results in over-engineering. |
| Applicability of the approach                                       | Applied to anything (specs, implementations, formatting, etc.)                                                     | Applied to only implementations                                                                                                               |
| (Culture) Removes tiring work from end-user engineers               | Ensures quality if engineers write lazily implementations.                                                         | Actively removes work.                                                                                                                        |
| (Culture) Removes fun work from end-user engineers                  | Little difference, though can feel meddlesome if very opinionated.                                                 | Actively removes work.                                                                                                                        |
| Affects security / bug risk (likelihood, blast-radius, time-to-fix) | Decreases likelihood. No change to blast-radius. Decreases time-to-fix, if paired with automated code refactoring. | Can decrease likelihood. Increases blast-radius. Decreases time-to-fix, if consumers can automatically bump versions.                         |
| Amount of code duplication                                          | Little difference - can be viewed as increased as code becomes more uniform.                                       | Decreases as it actively centralises code.                                                                                                    |
| Aids in [automated code refactoring](automated-code-refactoring.md) | Actively aids as code is more uniform and uses standard tooling familiar to automated code refactoring tools.      | Helps and hinders, as there is less code to refactor, but custom abstractions need custom automated code refactoring recipes.                 |

Some may also consider the _lifetime_ of the implementation affects the choice between standards and abstractions. A
long-lived implementation (or one with an unspecified lifetime) may benefit from one solution over the other. Likewise,
one may be more beneficial than the other for short-lived implementations or iterate development. In either case, both
standards and abstractions are applicable equally, since both standards and abstractions can be developed, iterated on
and thrown away as needed. For explicitly short-lived implementations, total maintenance cost is simply valued less, as
we know we will not need to maintain the solution.

## Context & Analysis

As of 2025, in the age of cloud computing and a rich engineering ecosystem (lots of mature languages and tooling):

- The implementation cost of building software is extremely low thanks to decades-worth of standards and abstractions
  being made - granted some systems still have a high implementation cost.
- Consistent _quality_ in implementations is difficult to achieve without opinionated standards (e.g. code analysis
  checks / linting rules).
- Many large systems that would benefit from standards or abstractions are now build as microservices, which is an
  architectural pattern which involves accepting the trade-off of increased code duplication due to decentralisation.
- Lot of existing organisations (Apache, CNCF, Google, VMWare (Spring), Microsoft, Other open source communities, etc.)
  dedicate their time and resources building abstractions and industry standards to make it easier for everyone to build
  software. If features or the development experience is lacking, the ecosystem is either abandoned and you need to
  switch or the features / development experience will be improved, you simply need to wait.

Taking this context into account, along with the above trade-off we can see that the areas where abstractions really
shine (decreasing implementation cost and code duplication) are valued much less than they might have once been valued.
Instead, the collective value of the benefits of standards outweigh the value of the benefits of abstractions, in most
situations - especially in the modern software engineering industry where abstraction improvements are just around the
corner.

In situations where implementation cost is high or code duplication is undesirable, abstractions are especially useful
and are more beneficial than standards (though adopting standards in these abstractions will aid code quality).

Since for some cases, abstractions work better, and for others, standards work better; we can construct a methodology
that gives guidance on when to use abstractions and standards.

## The resulting methodology

For components or systems that are composable/reproducible and fit into one or more of the following:

- high complexity exists and doing it "right" only has 1 or 2 options,
- doing it wrong is very unsafe / too risky,
- they are deliberately custom to a specific component or use case or system architecture, which is unlikely to change,

then it is very useful to create libraries/abstractions to increase safety and hide unnecessary customisations and/or
complexity. To manage these abstractions, create a small software [commons](https://en.wikipedia.org/wiki/Commons) for
the organisation and be vigilant with what is part of it.

For everything else: it is more useful to standardise and provide reference implementations / samples of what "correct"
looks like, and build these standards on top of existing industry standards and open source/purchasable abstractions.  
I recommend creating these standards _with_ engineers, to create buy-in and a more inclusive and collaborative
engineering culture - and avoid "top-down decision-making" of these standards.

By doing this, we create the largest amount of engineering ease whilst minimising the biggest costs.

## Applying the methodology

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

Congratulations, you're a pretty special case, and your literal offering are **abstractions**. Any further abstractions
that are highly-related to your offering, or could also be reasonably offered and maintained are probably worthwhile
investments.

However, nearly everything _behind_ that abstraction should be simply **standardised** and custom abstractions avoided,
except for things that are complex, unsafe or very deliberately custom.

Take advantage of existing abstractions offered the likes of Apache, the CNCF, Mozilla, Google, VMWare, Microsoft, and
many other open-source and proprietary Software Organisations - you might even be one of these people.

### For Product Organisations

Nearly everything you build would benefit from being **standards** rather than **abstractions**. Abstractions are useful
for complex, unsafe or deliberately custom components, or if there is something directly offered to users to improve
your "unique selling point" (USP), and can be made part of a limited organisational commons.

> "We're not Google or the CNCF, so we shouldn't try to be unless we can invest in people, time and tooling. Instead, we
> should _use_ abstractions - and if they're lacking in the functionality we want, then we can contribute them."

Take advantage of build upon the existing abstractions offered the likes of Apache, the CNCF, Google, Microsoft, VMWare,
and many other open-source and proprietary Software Organisations.

Creating custom abstractions / libraries to make things easier, or more reusable/less repeated is a low priority, as
covered by the above [trade-offs](#the-trade-offs) and [resulting methodology](#the-resulting-methodology). When
higher-priority work is complete, we can consider creating custom abstractions the areas where they make software
engineering easier, but even in this scenario we might not want to do it because we don't want to manage our own custom
stuff or hide standardisations behind custom interfaces. Instead, we could contribute to existing abstractions
(frameworks, libraries, virtualization technologies, etc.).

If we have exhausted our options and have the people, time and money to produce our own custom abstractions, then making
them makes sense - though we can still make these open-source to improve learning and inclusivity and so that these
abstractions can become industry-standard - at which point the Product Organisation is becoming a Software
Organisation (at least partly).

## Real-world examples

Here are some real world examples of the above [philosophy](#the-resulting-methodology) put into practice in modern
software organisations:

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
	  also actively impaired the knowledge, learning and critical thinking of engineers as it hid vital knowledge
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
