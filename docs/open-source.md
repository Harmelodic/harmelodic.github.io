---
hide:
  - navigation
---

# Open Source

## Contributions

As well as having [personal open source projects](#personal-open-source-projects), I've also given some minor
contributions to non-personal open source projects.

These usually take the form of reporting bugs or raising other issues, documentation fixes, the occasional bug fix, or
updating a project to support the latest version of a language / framework / other library.

Projects I've contributed to:

- [actions/setup-java](https://github.com/actions/setup-java)
- [apache/maven-enforcer](https://github.com/apache/maven-enforcer)
- [bepaald/signalbackup-tools](https://github.com/bepaald/signalbackup-tools)
- [bridgecrewio/checkov](https://github.com/bridgecrewio/checkov)
- [Cog-Creators/Red-DiscordBot](https://github.com/Cog-Creators/Red-DiscordBot)
- [corretto/corretto-docker](https://github.com/corretto/corretto-docker)
- [DaftLimmy-zz/Swearing-Xylophone](https://github.com/DaftLimmy-zz/Swearing-Xylophone)
- [FrameworkComputer/SoftwareFirmwareIssueTracker](https://github.com/FrameworkComputer/SoftwareFirmwareIssueTracker)
- [gaul/modernizer-maven-plugin](https://github.com/gaul/modernizer-maven-plugin)
- [googleapis/java-bigquery](https://github.com/googleapis/java-bigquery)
- [googleapis/java-pubsub](https://github.com/googleapis/java-pubsub)
- [GoogleCloudPlatform/prometheus-engine](https://github.com/GoogleCloudPlatform/prometheus-engine)
- [hashicorp/terraform-provider-google](https://github.com/hashicorp/terraform-provider-google)
- [hassansin/node-dnsbl-lookup](https://github.com/hassansin/node-dnsbl-lookup)
- [homerchen19/github-file-icons](https://github.com/homerchen19/github-file-icons)
- [istio/istio](https://github.com/istio/istio)
- [jaegertracing/jaeger-operator](https://github.com/jaegertracing/jaeger-operator)
- [jigna-shah/quest-fullstack-dev-JS-apprentice](https://github.com/jigna-shah/quest-fullstack-dev-JS-apprentice)
- [jigna-shah/quest-fullstack-dev-JS-Journeyman](https://github.com/jigna-shah/quest-fullstack-dev-JS-Journeyman)
- [kubernetes-sigs/kustomize](https://github.com/kubernetes-sigs/kustomize)
- [leereilly/games](https://github.com/leereilly/games)
- [MarshallOfSound/Google-Play-Music-Desktop-Player-UNOFFICIAL-](https://github.com/MarshallOfSound/Google-Play-Music-Desktop-Player-UNOFFICIAL-)
- [mega-manipulator/mega-manipulator.github.io](https://github.com/mega-manipulator/mega-manipulator.github.io)
- [microsoft/mssql-jdbc](https://github.com/microsoft/mssql-jdbc)
- [nadimkobeissi/appleprivacyletter](https://github.com/nadimkobeissi/appleprivacyletter)
- [open-telemetry/opentelemetry-java-contrib](https://github.com/open-telemetry/opentelemetry-java-contrib)
- [open-telemetry/opentelemetry-java-instrumentation](https://github.com/open-telemetry/opentelemetry-java-instrumentation)
- [opentracing-contrib/java-spring-jaeger](https://github.com/opentracing-contrib/java-spring-jaeger)
- [pact-foundation/pact-jvm](https://github.com/pact-foundation/pact-jvm)
- [projectlombok/lombok](https://github.com/projectlombok/lombok)
- [rust-lang/book](https://github.com/rust-lang/book)
- [spring-projects/spring-framework](https://github.com/spring-projects/spring-framework)
- [testcontainers/testcontainers-java](https://github.com/testcontainers/testcontainers-java)
- [togglz/togglz](https://github.com/togglz/togglz)

## Personal open source projects

My personal open source projects are found on [my GitHub](https://github.com/Harmelodic?tab=repositories).

There are a variety of different projects that have - some of which are highlighted and explained below.

### Init projects

It's helpful to have a starting point to get going. It's also useful to have that starting point be based on industry
standards. It's also useful to have those starting points showcase one's software engineering abilities.

To solve all that, I made my init projects.

- `init-web-frontend`: For guidance on building web frontends.
- `init-microservice`: For a template on building microservices in Java (my go-to "backend" language).
- `init-microservice-go`: For a template on building microservices in Go.

### Config projects

When switching between machines (work, home, laptops, etc.) it's helpful to carry over configuration of things between
machines. It's also nice to share that with other people, if they're interested in it.

My config projects cover this:

- `.code`: For configuration for Visual Studio Code.
- `dots`: For common scripts and POSIX config.
- `renovate-config`: For a common configuration for keeping other repositories up to date.

### Personal platform

Developing platforms involves multiple different components that are (hopefully) well-structured and fit together. I
have showcased how I tend to think about and structure platforms in the various repositories that form my "personal
platform".

The platform is built on Google Cloud Platform (GCP). This is because I'm most comfortable with it from my professional
experience, but also I like the way that Google Cloud structures projects, IAM and resources (as it is structured in the
way that I think of how infrastructure should fit together).

This infrastructure / platform is written in Terraform. Declarative language which maps a `resource` in code to an
infrastructure resource created which inherently discourages programmatical & conditional logic (which would otherwise
tend to lead to unnecessary complexity and differences between environments). Environments are easy to implement with
Terraform workspaces. Terraform "state" tracks what infrastructure "exists" and what doesn't, and the lack of continuous
reconciliation means stateful infrastructure issues (which inevitably happen) can be manually fixed easily (or at least
more easily than fighting against reconciliation system).

The infrastructure repositories are _ordered_ in a way that allows recreating the platform easily, without requiring
re-running old infrastructure, and provides clear structure and scoping of different platform components. As a rule,
infrastructure in any one repository should not depend on infrastructure created in a "later" (or equivalent)
repository - abiding by this rule completely avoids circular dependency issues.

The repositories that make up my personal platform are:

- `automation-infrastructure`: The first bits of infrastructure that enable creating further infrastructure using some
  kind of CD system (GitHub Actions, Atlantis, etc.). This infrastructure repo only has a single "environment".
- `personal-initial-infrastructure`: The base infrastructure of the personal platform. This is limited to infrastructure
  that is required for the specific platform, but is shared across all environments (e.g. artifact storage). This
  infrastructure repo only has a single "environment".
- `personal-env-infrastructure`: This is the base infrastructure of an environment in the personal platform. Typically,
  it contains some kind of compute runtime system, databases, networking, storage, and more. Ideally, only one
  environment is needed (production/prod), but sometimes other environments are needed for development, testing,
  research & development, or just for playing around in. In simple platforms, this can be contained to a single repo. In
  more complex platforms, I recommend splitting this up by "platform component" with a single underlying "base"
  infrastructure repository.
- `personal-apps`: Once a runtime platform exists, you need to deploy some apps (typically). This repository (or
  repositories) are environment-aware and provide deployment configuration for deploying applications the
  compute/runtime platform.

Need more platforms? Just add them! I like to _prefix_ repositories with their "platform" name. Ultimately platforms
"host" things, so it's useful to organise your platforms on the purpose of what your hosting: Business services, CI/CD,
Data Analysis, Development Tools, Corporate systems.

### Et Cetera

There's other stuff that I work on or try to build (and sometimes fail at building). Take a look
at [my GitHub](https://github.com/Harmelodic?tab=repositories) for more of that.

