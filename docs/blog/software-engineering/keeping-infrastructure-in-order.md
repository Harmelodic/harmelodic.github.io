# Keeping infrastructure in order

> Originally published: 09 June 2024

In this post, I talk about organising infrastructure code (e.g. Terraform code) to keep it highly maintainable and
reusable, with references to my personal setup. There is a diagram at the end for visual reference.

## Understanding layers

When creating a platform in the modern day, we often find ourselves doing so
through [Infrastructure as code](https://en.wikipedia.org/wiki/Infrastructure_as_code). For me, this has pretty much
always been through writing [Terraform](https://www.terraform.io) code. As such, I'll be referring to Terraform a lot -
however the principles I highlight here should be cross-language compatible.

Like with all code, it's quickly becomes apparent that breaking up our infrastructure code to keep the code easy to
maintain. You can initially do this with files & directories but eventually trying to execute a huge repository of
infrastructure code takes a very long time to run, which is painful - especially so, when you might have only made a
very small change.

Incidentally, organising Terraform code into directories and referencing them as Terraform Modules actually goes against
the design of Terraform's module system - modules are designed to be reusable, where we can create instances of modules
to create infrastructure, but sorting all your DNS-related infrastructure into a `dns` folder would result in a single
`dns` module to create that infrastructure. This will lead to increased confusion and code-complexity.

Eventually, you'll reach the point where you want to separate your infrastructure into separate Git repositories.

With application code, this is usually quite easy to do since the application will be self-contained. Infrastructure
code is a little different however. The infrastructure it creates is not self-contained - it depends upon some
underlying infrastructure to exist. Infrastructure upon infrastructure, leads us to building infrastructure in layers,
where each layer builds on the last. These layers are how we should split up our infrastructure code.

If we structure our Terraform code correctly in these layers, then creating our infrastructure is extremely easy: Start
with the repository creating the bottom layer, and move your way up.
The best benefit of this is being able to reuse our infrastructure code to create new environments of whatever
platform/infrastructure we're making.

Rule to remember: Never depend on infrastructure that is created in a layer above, because it won't exist yet!

## The bottom layer & automation

If you're using Terraform, it's likely you're using a Cloud provider, or have some physical hardware with some software
installed on the hardware that providers an API for a Terraform provider to hook into. This establishes absolute
fundamentals that we can build on.

First, I recommend creating a repository for code that will create the infrastructure required to automate creating
infrastructure. It is a bad idea to run all infrastructure code manually, so establishing some initial "automation
infrastructure" that can then handle the CI/CD of further infrastructure code is a powerful thing. Since I use GitHub
Actions to run my infrastructure,
my [automation-infrastructure](https://github.com/Harmelodic/automation-infrastructure) creates the absolute minimal
infrastructure that is needed to enable GitHub Actions to execute Terraform code for me.

Once this has been made, all infrastructure created can be executed automatically, rather than manually.

## Understanding Platforms

Before we go build infrastructure to actually run the software we're developing, we need to think about what goes into
developing software. There is:

- Software Engineers working with code, documentation, bug & issue tracking.
- CI systems to build that code into software.
- A "business-runtime" platform to run that software.
- CD systems to deploy that software to the "business-runtime" platform.

Each one of these aspects of developing software requires some sort of infrastructure. For some, we could buy some
Software-as-a-Service (SaaS) (e.g. GitHub for storing code). For others, we may want to insert our infrastructure to
either support existing software (e.g. our own servers for running GitHub Actions). For everything else, we can build
infrastructure specifically for that purpose.

Separating this infrastructure, means we can build infrastructure tailored for each of these aspects of developing
software, whilst reducing the blast radius when issues occur. As such, each of these aspects of developing software
results in a "platform" being built for that purpose (or we buy it).  
At a small scale, such as a individual or a startup, these concerns are less important, often resulting in these
platforms being merged into one big platform (plus purchasing SaaS products).

Ultimately, this informs us of the next layer of repositories that we need to make. The initial, pre-environment
infrastructure needed for each of these platforms. This is usually some sort of space defined in your Cloud setup for
the platform and a place to store software artifacts for that platform that can be deployed to different environments.

You can find an example of this in my personal setup with
my [personal-initial-infrastructure](https://github.com/Harmelodic/personal-initial-infrastructure) for creating
infrastructure for my *Personal* platform.

## Environments

Before we create further infrastructure for each platform, we probably want to have multiple *environments* in each
platform. A "production" (or "prod") environment where users & systems interact with the released software, and other "
production-like" environments for development needs (playgrounds, manual testing, etc.).

These environments must be effectively identical to one another, for them to have any value as "production-like"
environments. Only small differences like the name of the environment, size of servers and basic IAM/Firewall
restrictions should be allowed as differences between the environments.

Up until now, the infrastructure code we've written has been very straightforward: either the infrastructure exists or
it doesn't. In order to support environments, we need to introduce another dimension into our infrastructure code:
Templating. Templating allows us to construct our infrastructure code with variables in it. When we want to create
infrastructure for a particular environment, we simply fill the variables with the appropriate values, and execute the
infrastructure code.

In Terraform, we can do with [Terraform Workspaces](https://developer.hashicorp.com/terraform/language/state/workspaces)
and variables. I recommend having a 1-1 mapping between Terraform Workspaces and Environments, then using variables to
define aspects of the envionment allowed to be configured differently in each environment.

Rule to remember: All infrastructure code that will create infrastructure in a platform environment MUST be written in
a "templated" way. This is to ensure that creating environments actually works.

For small platforms, we can often be successful with just a single repository covering all platform infrastructure in an
environment.

For larger platforms, it is likely that breaking up infrastructure into further layers within an environment becomes
useful, where each layer in the environment has it's own repository.

You can find an simple example of a how a platform environment repository looks and works by looking at
my [personal-env-infrastructure](https://github.com/Harmelodic/personal-env-infrastructure) repository.

## Further layers in larger platforms

As mentioned above, larger platforms usually end up needing further breaking up of infrastructure code to keep the code
maintainable.

I don't have this in my personal setup, but have built these professionally. The following is an example structure of
how you might split the layers/repositories:

- `{platform}-management` for creating projects* & project*-level IAM permissions.
- `{platform}-networking` for creating DNS and networking infrastructure inside projects.
- `{platform}-kubernetes` for creating a central Kubernetes cluster for your platform (assuming you only need 1 cluster
  per environment).
- `{platform}-service-accounts` for creating all the service accounts required for the applications that you plan on
  deploying (do this centrally aids in governance & issues with service account name uniqueness).
- and several deployment repositories (see below).

In my personal setup, I don't have these splits, as it is a very small scale setup.

*By "project", I mean GCP Project. Other clouds have their equivalent resource management concept.

## Deployment repositories

As the industry continues to embrace GitOps, we are finding ourselves deploying software by building a new revision of
the software, and then updating code to deploy that revision.

In order to provide separation between logical groups of deployed software, we can have a dedicated deployment
repository, for each logical grouping, that contains the code that defines how all the software should be deployed.

This software that is being deployed, will likely depend on some infrastructure being in place (e.g. Databases, Storage
Buckets, Message Busses, etc.).
As such we could bundle the infrastructure code needed by that software, with the deployment code for the software into
a single Deployment Repository.

You can see an example of a Deployment Repository if you look at
my [personal-apps](https://github.com/Harmelodic/personal-apps) repository. However, note that *Argo CD* (a piece of CD
software) is configured in this repository. Deploying your CD software to the same platform as your business-runtime
applications has trade-offs, and many folks may find having a separate CD platform as more preferable.

Note: for the "business-runtime" platform mentioned above that will run the product software for an organisation, I
recommend doing some [DDD](https://en.wikipedia.org/wiki/Domain-driven_design) work to figure out the bounded-contexts
and subdomains, and organise these Deployment Repositories based on those results.

## Diagram

<img src="https://storage.googleapis.com/harmelodic-web-static-prod/blog-images/infrastructure-layers.png" alt="Infrastructure Layers Diagram" style="width: 90%;">

Hope this helped!

~ Harmelodic
