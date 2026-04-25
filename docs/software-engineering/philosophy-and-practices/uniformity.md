# Uniformity

Maybe there's a more official / industry-standard term for this, but uniformity is the term I'm using for the overall
"sameness" that many organisations, managers, engineers (etc.) try to achieve when doing software engineering.

Uniformity could have different scopes (e.g. industry, organisation, domain, department, team, systems). However, for
the purposes of this document, I'm referring to organisation-wide uniformity.

Uniformity appears in the following functions:

- Product Design
- Architecture
- Tech Stack
- Code
- Tooling & Setup
- Ways of Working

The general advantages and disadvantages / arguments for and against ...

- Uniformity affects both social & technical sides of engineering.
- Uniformity affects both the means and the outcome
	- Maintainability
	- Ease of development
	- Fun of development
- Uniformity creates efficiency.
- Lack of uniformity creates space for flexibility to solve a problem with a bespoke, creative, innovate and/or fun
  solution.
- Get too uniform & ignoring domain-needs or individual needs & interests and your getting a bit fascistic and going to
  breed contempt, suppress creativity & fun, and people will just do Shadow IT.
- Get too loose with uniformity, and you end up with an inefficient, unsustainable and expensive mess that eventually
  will also not become fun to be productive in.

Typically, I see some people (management, platform engineers, some architects) want lots of uniformity because those
roles can & often lean into "controlling the chaos". I also see lots of engineers/developers and some architects yearn
to be free from uniformity, so they can do the work as they see fit. This showcases one of the ways that uniformity
affects the social side of software engineering, but also highlights a power-dynamic to how uniformity is manifested.

My take on handling uniformity as a concept / approach:

- Having uniformity in _some_ areas is incredibly powerful and enabling and so should be used (to a degree), but done
  so transparently and with clear structure & process.
- Since software engineering is a sociotechnical activity, we must consider the social aspects of uniformity seriously
  (I find they are often overlooked or dismissed). This practically means:
	- When faced with controversial topics regarding uniformity, the dialogue & decision-making process should be
	  delicate where everyone is encouraged to have an open mind, and time is taken for everyone to air their
	  perspectives. Don't avoid the conversation, because that's avoiding the problem and letting it fester. Don't
	  tackle too harshly or heavy-handed, because that's going to breed resentment. Transparency, trust, and a robust
	  decision-making process is key.
	- Building in flexibility in how uniformity is conducted through different strictness levels of uniformity, and
	  allowing for exceptions to uniform.
	- Actively build [alignment](./alignment.md) around how uniformity is viewed & handled in different functions.
	- Enable and reap the benefits of uniformity, by creating or using things that aide
- Different strictness levels for prescribing uniformity:
	- Standards
	- Guidelines
	- Conventions
	- Exceptions made to standards & guidelines should be documented and facilitated through a transparency and
	  accessible decision-making process.
	- Conventions are optional by definition, but it should be easy to adopt and alter a convention to be bespoke to a
	  domain / team / system.
- Different concepts for enabling uniformity without prescription: Libraries, Tools, Docs, Patterns, Recommendations,
  Linters / Formatters.
- Maintain/Modify implemented uniformity through: [alignment](./alignment.md) conversations & processes, defined fitness
  functions.

My take on handling uniformity in the different functions:

- I recommend taking a balanced approach, err-ing on the side of uniformity for many things, with notable exceptions.
- Business Domain-focused parts of solutions should avoid being uniform with everything else. This way, they can be free
  to actually focus on solving the business problem. This practically means:
	- A general rejection for canonical modelling.
	- System architecture (actual, not patterns) should be as complex as the problem needs it to be.
	- Product Design should be as unique, creative, fun, interesting as it needs to be or has humans want it to be.
	  Uniformity (or lack thereof) in Product Design is a design choice, and should be left to the org/product people to
	  decide how it should be.
	- Usage of Domain-driven design (DDD).
	- If a particular domain requires a very specific tech stack, then use that tech stack - unless the purpose of the
	  endeavour/project is to create a viable alternative tech stack for the domain.
- Uniformity on Tech Stack is otherwise generally a sensible and uncontroversial thing - but should be uniform per
  ecosystem (Frontend, Backend, Platform, Data, etc.).
	- Should mainly standardise on things like language, framework, build tool, key/common libraries - and should use
	  industry-standards (not in-house developed abstractions).
	- Some in-house developed tech is useful, but usually only stuff that helps:
		- integrates industry-standard tech together (e.g. Spring applications and Prometheus)
		- extends (not abstracts) existing industry-standard tech to improve the developer experience for complicated or
		  complex "boilerplate" code.
	- One example Fitness Functions for this: The complexity and amount of CI (build) pipeline code is low.
	- The Platform tech stack should be pretty uniform (e.g. networking, hosting, observability, available cloud
	  services, etc.) whilst allowing for extension where needed by Domain needs.
- Uniformity of code is often controversial, but generally:
	- Package structure should be either package-by-feature or package-by-layer (preferably package-by-feature).
	- There is a "good-enough" centrally-defined code style (per language) that all applications inherit, but is
	  overrideable by domains/teams (still ensures linting/formatting but provides optionality). If any domain/team
	  significantly drifts from the centrally-defined code style, it should be investigated as to why that's happening
	  (could be fine, could be not)
	- Leave the rest to engineers to handle, and let conventions organically grow and die over time.
- Uniformity of Tooling & Setup:
	- Some things (e.g. VPN client, security software) will be mandated for security reasons.
	- Otherwise, most engineers has their own way of doing things.
	- There should be a company convention to assist onboarding and developer experience for those who are happy with
	  the company convention.
	- I really don't like it when metrics and data is gathered from machines for the company for analysis. Even with the
	  best intentions, that's just plain spying and becoming data-obsessive.
	- Not counting Observability tooling or things like that in here, that's part of Tech stack.
- Uniformity of Ways of Working... Generally, I think:
	- Some methods are generally better than others:
		- Domain-driven design (DDD) is good, so I encourage an operating model that organises by business domain into
		  cross-functional teams (to better create a close collaboration between software engineers and business/domain
		  experts).
		- Agile / Iterative development practices.
		- DevOps approach (you build it, you run it)
		- A bit of scrum-like ceremonies to provide some "project management"-like structure to enable the team.
		- Adopting practices & a mentality that working in the team a fun, open, safe space to produce software you feel
		  proud of and have confidence in.
	- However, actual ways of working is super dependent on business domain, team dynamic, individual personalities,
	  management styles and the organisation's operating model, and so I prefer to treat those things as a guideline or
	  convention.

"Choose Boring Technology" by Dan McKinley is an interesting read. Not saying I advocate for that mentality or way of
thinking, but some things I think are aligned with that piece.
