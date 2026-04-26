# Uniformity

Maybe there's a more official / industry-standard term for this, but uniformity is the term I'm using for the overall
"sameness" that many organisations, managers, engineers (etc.) try to achieve when doing software engineering. Another
term could be "consistency"?

Uniformity could have different scopes (e.g. industry, organisation, domain, department, team, systems). However, for
the purposes of this document, I'm referring to organisation-wide uniformity.

Uniformity appears in the following functions:

- Product Design
- Architecture
- Tech Stack
- Code
- Ways of Working
- Developer Machine Setup

The general advantages and disadvantages / arguments for and against ...

- Advantages:
	- Uniformity can broadly create "efficiency" which allows software to be made and maintained easier and faster.
	- Uniformity can make maintenance easier.
	- Uniformity of systems & code can make systems more intuitive from an _engineering_ perspective, which can make
	  onboarding and collective ownership easier.
	- Uniformity can make it easy to create a new thing that "fits in".
	- Uniformity can make it easier to build integrations between different things that are also uniform.
	- Uniformity can make it easier to change from one uniform solution to another.
	- Uniformity can help make software engineering more predictable & therefore easier to plan & forecast.
	- Uniformity in different solutions can make the user experience more intuitive.
	- Uniformity in less-valued areas can mean we spend less time supporting multiple things and more time doing
	  valuable things.
- Disadvantages:
	- Lack of uniformity can create space for flexibility to solve a problem with a solution that is fit for purpose,
	  creative, innovate and/or fun.
	- Uniformity can contribute to making the job boring / robotic (assembly-line).
	- Social uniformity opposes social diversity (morally not good)
	- Uniformity can be a resilience / security concern as it allows exploiting real vulnerabilities to be simpler and
	  more automated and therefore more dangerous (careful: the answer to this is not "security through obscurity").
	- Too much uniformity in systems & code make systems less intuitive from a business domain perspective (system
	  should scream their intent / purpose (ref: Robert C. Martin)).
	- Uniformity can feel restrictive, paternalistic and controlling.
	- Uniformity can breed a "feature-factory" mentality, especially when maintenance & operations work lack attention.
	- Uniformity in areas where uniformity has little value gives the illusion of efficiency, but doesn't _actually_
	  produce much (or any) efficiency.
- Warnings / Be aware of:
	- Uniformity choices affects both social & technical sides of engineering.
	- Uniformity choices affects both the means and the outcome of software engineering.
	- Get too uniform & ignoring domain-needs or individual needs & interests and your getting a bit fascistic (harsh
	  description maybe, but still) and leads to a toxic and unhelpful practices (contempt, suppressing creativity &
	  fun, Shadow IT).
	- Get too loose with uniformity, and you end up with an inefficient, unsustainable and expensive mess that
	  eventually will also not become fun to be productive in.

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

My take on handling uniformity in the different functions (details of the specific things that I think are "good" is
detailed more in the rest of this [philosophy and practices](./index.md) documentation):

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
	- The Platform tech stack should be pretty uniform (e.g. networking, hosting, observability, VCS, CI/CD, CMS,
	  supported cloud services, supported database/cache/messaging systems, etc.) whilst allowing for extension where
	  needed by Domain needs.
- Uniformity of code is often controversial, but generally:
	- Package structure should be either package-by-feature or package-by-layer (preferably package-by-feature).
	- There is a "good-enough" centrally-defined code style (per language) that all applications inherit, but is
	  overrideable by domains/teams (still ensures linting/formatting but provides optionality). If any domain/team
	  significantly drifts from the centrally-defined code style, it should be investigated as to why that's happening
	  (could be fine, could be not)
	- Leave the rest to engineers to handle, and let conventions organically grow and die over time.
- Uniformity of Ways of Working:
	- I think some methods are generally better than others:
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
	- On tooling for ways of working, I think...
	- Most "ticketing" software is fine. Jira is basically industry-standard. Most people hate Jira because their
	  organisation enforces uniformity and Jira had a lot of functionality (which makes it feel complicated or
	  bloated) - however, I think this goes for any ticketing software and the lesson should be: "don't have strict
	  uniformity and don't try to be clever or complicated with it" rather than "don't use Jira".
	- Miro and Draw.io/diagrams.net are decent whiteboarding options. If using Miro, try to not create lots of Miro
	  boards, and stick to one board per business domain things, and one board for platform stuff.
	- I'm not a huge fan of UML or UML-like / Architecture tools for Solution/System architecture modelling, let
	  alone requiring uniformity for them. Why? Because:
		- 2 major problems occur:
			- It is a lot of effort to maintain, we get drift between what we model and what the systems actually
			  look like which makes the models much less useful and relevant.
			- Viewing how the system architecture is useful from time to time, but most engineers just "know" how
			  their system is architected, and where they don't know, they'd rather know what actually exists than
			  what is modelled to exist.
		- The combination of these problems means that people don't find it super valuable, which provides less of
		  an incentive to keep it up to date, which further decreases its value. These models then become either a
		  lot of work to maintain, producing limited value.
		- Historically, we've leaned into "Architect" roles maintaining and managing this, which unfortunately then
		  often leads to ivory tower architecture (where the architects care more about what is modelled and how it
		  can be controlled & remodelled, than what actually exists and is needed for the business domain needs).
		- Requiring uniformity of Architecture tooling tends to lead to canonical modelling (I don't think that's
		  good).
		- I see modern Solution/System Architecture often instead being done in whiteboarding tools and in decisions
		  being formed, made and recorded in ADRs (Architectural Decision Records). Then we utilise Platform tooling
		  like Service Mesh visualisation tools, Tracing tools and Contracts (from Contract Testing) to then
		  visualise the system architecture that _actually_ exists (when we need that visualisation).
		- In this operating model, Architects therefore reduce their focus on ivory-tower architecture modelling and
		  instead focus on facilitating design conversations and providing advice & guidance on ADRs.
		- The whiteboarding and ADRs should be uniformly handled within a business domain at least, if not
		  organisation-wide.
	- I'm not too opinionated on the uniformity of Architecture tooling for Business/Enterprise Architecture:
		- Generally, I like doing event-storming and other DDD-like exercises to map/visualise how business
		  domains and processes work.
		- This is most effectively done in-person on real whiteboards, but getting them into a software tool is
		  useful as a reference.
		- If that's all in the same tool or not, I don't really care too much - but I'd at least recommend using the
		  same tool for an entire business domain.
	- I think documentation comes in 4 forms and should be uniformly handled within their scope, though I generally
	  prefer handle docs as code (Markdown + extensions/plugins):
		- Domain documentation - docs for both business operations/processes/terms and system-related things for
		  that domain (e.g. a simple reference system architecture diagram, some runbooks, etc.). Should be put in a
		  single documentation location for the domain.
		- Code Repository documentation - docs specific to a single code repository. Should be kept in the code
		  repository, as close to the relevant code as possible (could just be code comments in many cases).
		- Team documentation - docs for how the team operates. Should be kept in a single location that the team is
		  happy with.
		- Purely technical documentation - cross-functional docs re: platform, ecosystem docs, standards,
		  guidelines, conventions, and onboarding. Should be kept in a single location for everyone to view (e.g.
		  stored in a git repo, viewable in a Developer Portal).
- Uniformity of Developer Machine Setup:
	- Some things (e.g. VPN client, security software) will be mandated for security reasons.
	- Some things (e.g. what laptops & operating systems can be used) will be mandated for security & maintenance
	  reasons.
	- Otherwise, most engineers have their own way of doing things and should be left to the developer to decide that.
	- There should be a company convention to assist onboarding and developer experience for those who are happy with
	  the company convention, or who lack knowledge of how to configure their machine in different ways.
	- I currently recommend using [mise-en-place](https://mise.jdx.dev/), since it works well, allows for installing
	  custom tooling & scripts too, and is quite GitOps-y, since everything is saved to `mise.toml` config files - so
	  you could have an organisation-wide `mise.toml` file as a "base" and then allow anyone to extend on that.
	- I really **don't** like it when metrics and data is gathered from machines for the company for analysis. Even with
	  the best intentions, that's just plain spying on people and becoming data-obsessive regarding how engineers
	  conduct their work (often ignoring engineers' preference). If we're interested in this from a management /
	  developer experience perspective, we should just ask/poll/survey engineers - that way we get the answer, and it
	  gives the opportunity to the engineer(s) to convey their preference.
	- Not counting Observability tooling (Grafana, Kibana, etc.) or things like that in here, that's part of Tech stack.
	- Not counting Project Management tooling (JIRA, Miro, etc.) or things like that in here, that's part of Ways of
	  Working.

---

["Choose Boring Technology" by Dan McKinley](https://mcfunley.com/choose-boring-technology) is an interesting read
that I mostly agree with. Though I'd say most business don't even need 3 "innovation tokens"
