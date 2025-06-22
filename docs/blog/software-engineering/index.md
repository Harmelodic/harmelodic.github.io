# Software Engineering Overview

Currently just a bunch of various articles on Software Engineering-related topics.

Will reorganise this at some point.

## Thinking about re-organising

Basically make a little engineering handbook for myself.

Have a section for general software engineering philosophies and topics:

- **Software practices** (Agile, Waterfall, DDD, Facilitating Software Architecture, Mob programming, XP)
- **Organising Code** (Monorepo vs Multirepo, Application repos, Deployment repos)
- **Contracts / Specifications** (interfaces, abstractions, APIs, testing)
- **Architecture** (System design, modelling, patterns trade-offs)
- **Testing strategy** (Unit/Integration/Integrated, Mocks & Stubs, Running locally, TDD, environments (ironically),
  shift-left, the test data problem)
- **Development practices** (Version Control, Package management (+SemVer), IDEs, error handling, onboarding checklist,
  automate it, documentation (handbook, system docs), code style, Standardisation)
- **Operational work** (Monitoring & Alerting, SLIs/SLOs/SLAs, Troubleshooting)

Then organise based on different ecosystems of software development, with different featured stacks:

- **Platform**
	- General (Infra as code, zero trust, IAM access, environments, observability, DNS, networking)
	- Compute platform (compute options (serverless, kubernetes, VMs, hardware), service-mesh, ingress/egress, LBs,
	  certificates)
	- CI/CD / Supply-chain platform (git, build system, contracts, artifact storage, security analysis, deployment
	  systems)
	- Internal Development Tools platform (sourcegraph, developer portal, templating, documentation)
	- Big Data (storage, normalisation, access)
- **Backend**
	- APIs
	- Java
	- Go
	- Rust?
    - Databases / Storage (SQL/Relational, In-memory key-value, Document, Blob / File Storage)
    - Messaging (Queues, PubSub)
- **Web Frontend**
	- JS/TS, React, etc.
	- Other web frontend considerations
- **Data Science**
	- Python, SQL and R
	- Moving & Processing data (data pipelines, ETL)
	- Data Analysis & Viewing (in-place data-crunching, dashboards, jupyter notebooks)
	- Machine Learning / Artificial Intelligence
- **Mobile**
	- React Native
	- Swift / Kotlin
- **Desktop**
	- Linux: Qt, GTK
	- macOS: Swift / Universal Apps
	- Windows: Windows SDK
	- Electron
	- JavaFX
- **Embedded**
	- C/C++
	- Rust

Each development ecosystem should cover:

- Specific principles, architectural practices
- Operational work (Ops)
- Development lifecycle
- Language/Stack specifics (per language/stack)
	- Local machine setup
	- CI (validate, test, build)
	- CD (deploy, release)
	- Doing specific things in that language
