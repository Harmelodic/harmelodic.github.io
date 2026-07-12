# Tech Stack Selection

Selecting a Tech Stack for what software you want/need to build can seem daunting, but with a little knowledge of where
you should invest your time and money, and what realistic options are available, much of it ends up being decided for
you.

TODO - Expand On:

- Build vs Buy (or a mix of both).
	- What you buy: Best of Breed? 1 solution for everything? Best Suite of Things?
- Understand the relative importance of your business domains to help you understand what you should build or buy, e.g.:
	- [Core Domain Charts](https://github.com/ddd-crew/core-domain-charts)
	- [Wardley Mapping - value chains](https://en.wikipedia.org/wiki/Wardley_map)
- Cloud/SaaS vs on-prem:
	- Money Cost
	- Speed of development needed
	- Speed of scaling needed
	- Coupling / sovereignty
- For what you buy, think about:
	- Actual functionality offered, and your needs covered (and typical medium-future needs covered).
	- Quality of APIs
	- Support contracts
	- Legal compliancy
	- Geographic sovereignty (given unstable geopolitics or reducing dependency on the USA / EU).
	- Self-host or not (see Cloud/SaaS vs on-prem)
- For what you build, think about:
	- Open-source vs Proprietary, pros and cons
		- Cost - beware of
		  the [zero-cost fallacy of open-source](https://www.thoughtworks.com/insights/blog/open-source/zero-cost-fallacy-open-source-agentic-era)
		- Availability
		- Bug-fixing
		- Support
	- I usually promote usage of Open-source over proprietary
	- Unless you building embedded systems (and even then), you're probably going to build some backend web services.
	- Modern (as of 2026) choice of open-source backend ecosystems:
		- Java / Kotlin + Sprint Boot + Maven/Gradle
		- Go + many libraries
		- Node.js + TypeScript + Express
		- Ruby + Ruby on Rails
		- Python + Django/Flask
		- Read more about [my thoughts on backend](../../backend/index.md).
	- Also, see my init-microservice projects
