# Philosophy and principles overview

- **Architecture**
	- System design, modelling, patterns trade-offs, facilitating Software Architecture

- **Contracts and APIs**
	- interfaces, abstractions, APIs, libraries, semantic versioning, loop in tests

- **Development practices**
	- Version Control, Package management (+SemVer), IDEs, error handling, onboarding checklist,
	  automate it, documentation (handbook, system docs), code style, Standardisation

- **Operations**
	- Monitoring & Alerting, SLIs/SLOs/SLAs, Troubleshooting, but also: business operations? (loop in DDD and ways of
	  working)

- **Organising code**
	- Monorepo vs Multirepo, Application repos, Deployment repos

- **Testing**
	- Unit/Integration/Integrated, Mocks & Stubs, Running locally, TDD, environments (ironically),
	  shift-left, the test data problem

- **Ways of Working**
	- Agile, Waterfall, DDD, Mob programming, XP

- **Technical Leadership**
	- Technical advice & Guidance for engineers, makes sure technical things don't fall through the cracks, facilitates
	  high-level architecture, can make final technical decisions where there is conflict.
	- Often (but not solely responsible for): drives initiatives & conversations, identifies new/upcoming non-functional
	  requirements, identifies problematic technical areas that need prioritisation
	- Some organisational structures gives technical leadership more mandate, others require more democracy/process.
	- Existence provides a technical career path and helps prevent flat-hierarchical issues of "why do THEY get to do
	  that and not me?" whilst not necessarily being hierarchical (depending on organisational mandate).
	- Different from Eng Management as they are _expected_ to be technical experts, and are not expected to engage in
	  the people management side of engineering.

- **Engineering Management**
	- People / Administrative management focused side of engineering (not technical)
	- Hiring, career development, code of conduct, people-conflict resolution, organisational structure in order to
	  ensure workforce effectiveness and efficiency, drives and supports team in their "way of working" stuff
	  (project/team management), access & approvals, aids procurement of tools that would aid engineers in their work.
	- Aids planning efforts by bringing the organisational perspective to planning, creates projections based on
	  statistics/insights gained to aid the organisation project/predict timelines
	- Auditing regulatory & internal-policy compliance. Communicating with the relevant parties to prioritise the work
	  so that engineering compliance is fulfilled (DORA, system inventories).
	- Diplomacy throughout
	- Different from Technical Leadership as they are _expected_ to know how organisations & people work and how to
	  organise and motivate workers into ensuring work is done, and is not _expected_ to actually know how to do that
	  work or to even do any of it - in fact, doing so can often create internal political animosity, as engineers
	  (workers) usually want high-levels of autonomy and not have management "interfering", and knowing this is part
	  knowing how to motivate workers.
	- Include a "Software Organisation Management Lifecycle" page that tells the story of how an organisation goes from
	  1 (or few) to a large scale organisation. This would include:
		- What functions an organisation needs to fulfil (Management functions, how workers are organised, how systems
		  are organised)
		- How management gets created (what their purpose is)
		- Small organisation model (horizontal slicing for efficiency)
		- Medium organisation model (transition from horizontal to vertical slicing, plus admin departments)
		- Large organisation model (vertical slicing for focus, internally sliced as necessary, plus admin departments)
		- Huge organisations should probably not exist and should just be multiple large organisations (arguably the
		  same for large organisations)
		- Admin departments tend to scale better than product departments.
		- Top-down/bottom-up decision-making/power, and the preferences from each perspective (mgmt, workers)
