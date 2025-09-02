# Shipping Software

Shipping software involves two processes: Build and Release.

- Build is the word that covers the process of turning "code" into an "artifact".
- Release is the word that covers the process of making that "artifact" available for use to users.

Shipping often comes under other terms like Continuous Integration, Continuous Deployment (CI/CD), or the Software
Supply Chain.

> The term "shipping" has been adopted from the same term used to mean the transporting or sending of goods/cargo via
> literal ships (sea vessels).

## Build

Build processes take code and produce artifacts. What the code is intended for, you might expect a different artifact.

In a build process you tend to want to do:

- **Code Compliance**
    - Linting to ensure adherence to a code formatting / style guide / code structure rules.
    - Ensure code is modern (e.g. no deprecations and uses good modern languages practices).
    - Ensure zero known common bugs or code smells
    - Simple Security scanning (e.g. detecting leaked secrets, vulnerability detection)
    - Simple SBOM analysis (e.g. licensing rule-violation detection, dependency usage-violation detection, more
      vulnerability
      detection).
    - (More complex analysis can be done passively and continuously by an external system
      to [improve software quality](./improving-software-quality.md), these simple protections are in place to ensure
      the critical and basis issues are mitigated)
- **Compiling** - Turning the code into binaries, or other kinds of executable/loadable code.
- **Testing** - Running tests to give us confident the code is fit for purpose and bug-free. Can include:
    - Unit tests
    - Integration Tests
    - Contract Tests
    - Necessary or Simple Performance Tests and/or Benchmarks
    - _Zero_ Integrated Tests - not worth [The Shared Test Data Problem](the-shared-test-data-problem.md)
- **Packaging**
    - Bundling to create an artifact.
    - Signing the artifact.
    - Storage of the artifact.

Note: Producing new versions of artifacts for users to access (if the artifact itself is what the users use), is NOT
part of the build process. That is part of the [release](#release) process.

Example artifacts could be:

- Executables (e.g. Programs, Applications, Container Images).
- Code libraries for use in other projects.
- Deployment Manifests (e.g. Helm Charts).
- Infrastructure execution plans (e.g. Terraform plans).
- Specifications / Contracts (e.g. PACT contracts).
- Rules / Policies for use in analysing or linting other code.
- Generated code for documentation (or anything else).

Artifacts can sometimes be ephemeral (like _infrastructure execution plans_), but most artifacts are permanent and
should be stored in some kind of artifact repository or registry (different providers of artifact repository solutions
call them different things).

Since we haven't _released_ anything yet, the artifacts should be stored in an artifact repository for
development/pre-release/release-candidate artifacts, and not available in an artifact repository for releases.

Implementation specifics to the building different projects can be found in the specific sections
under [Software Engineering](../../index.md).

## Release

Release processes take artifacts and make them available to users.

Release processes generally always begin with the publishing of a built artifact to an artifact repository that is used
for storing releases. When publishing release artifacts, it is recommended to make artifacts immutable and not possible
to overwrite - this prevents a lot of Supply Chain Security issues.

Beyond publishing an artifact, release processes can look quite different depending on what it is your releasing.
This tends to come in 2 forms:

- Releasing new versions of a service that users access remotely (e.g. APIs or websites).
- Releasing new versions of an artifact (e.g. an application, a CLI program, a library, specifications, etc.).

Let's see what each one looks like:

### Releasing services

Services is the term I'm going to use for any released artifact where the end user is not using the artifact itself, but
just uses the _service_ provided by a running instance (or instances) of an artifact. These tend to be websites or
web-based APIs. The artifact is used by the software engineers who manage the operations of the service - these are
usually the same people who wrote the code and built the artifact.

Releasing services is often referred to as _deploying_, since in simple release processes, we are simply deploying a new
artifact to release a service. I will be using the term _release_ instead, since that's the word that I'm using for
everything else, and in advanced release processes, there is a difference between _deploying_ and _releasing_ a service.
More on this below.

Performing operations on services (like upgrades, monitoring, etc.) is made much easier by using a GitOps approach. This
means all configuration for running a service is stored and sourced from code as "manifests", and some other deployment
system takes care of syncing / reconciling the existing running service system, to match what is defined in the
manifests.  
Even in non-GitOps release processes, we tend to have some kind of release script, stored as code. that is executed to
perform a release. An artifact version or ID is then fed into this script in order to release that specific artifact as
the new version to use for the service.

A release process for services therefore looks like:

- Publish the release artifact (EXE, JAR, Container Image).
- Update the existing GitOps manifest (or script) to use the new version.
- Allow our deployment system to deploy the new version (by syncing or running a deployment script).
- In a simple release system, we can simply perform a blue-green release:
    - Deploy the new version of the service, and have both old and new versions running, but only have the old version
      doing processing.
    - Switch over the production processing from the old version to the new version.
    - Remove the old version of the service.
- In an advance release system, we could configure our deployment system to perform a canary release:
    - Deploy the new version of the service, and have both old and new versions running, but only have the old version
      doing processing.
    - Initiate the new version to start processing a % of the work needed (e.g. serving users or consuming events).
    - Evaluate whether the new version is successful at processing the % it is handling (rollback and alert an engineer,
      if not).
    - Continue increasing the % of processing and evaluating the success of the new version.
    - Once the new version is processing 100% of the work, then remove the old version.
- In other advanced release systems, we could do either a blue-green release or a canary release, but ALSO disable new
  functionality offered by the service behind a "feature flag", and release this new functionality (to a % of users or
  all users) at a specific chosen time.

As you can see, the act of _deploying_ a new version only covers part of the process. The act of _releasing_ a new
version can be as simple or advanced as we'd like, but the term "releasing" covers the entire process.

When publishing the release artifact, we must be able to identify which version of the artifact we have just released.
It is useful to be able to identify versions both by some simple, intuitive human-readable identify (e.g. an incremented
version number) and be able to identify and connect a version to the code update that produced that version (e.g. a Git
SHA hash).

Updating existing GitOps manifests (or scripts) can be done automatically by either an "update bot" (that searches for
new versions of deployed artifacts and updates them) or by making the update part of a post-publishing task. I tend to
recommend using the "update bot" method, as it decouples the publishing of a release artifact from the deployment
manifests that use it, and allows the deployment manifests to effectively be self-updating.

As of 2025, simple and advanced release processes can be implemented quite easily by using container images as your
artifacts, Kubernetes as your container-execution system, Argo CD (or other similar tool) as your deployment system, and
Argo Rollouts as a canary-release system. Feature toggling can be implemented in a variety of ways, but I tend to
encourage GitOps-feature toggling, by updating service configuration to enable features.

Implementation specifics to the releasing
of [Backend](../../backend/index.md), [Frontend Web](../../frontend-web/index.md)
or [Data Science](../../data-science/index.md) services can be found in their individual sections.

### Releasing artifacts

...?

Implementation specifics to the building any projects can be found in the specific sections
under [Software Engineering](../../index.md).

## Shift Left

Imagine the process of shipping software is laid out from left to right. Where new local development is the start on the
left, and a release is at the end on the right.

"Shift Left" is an approach to designing processes of shipping software, where we try to design and refine processes
by "shifting" more work to the left, so that it occurs earlier (or even in parallel) in the whole process.

The advantages of this means MUCH quicker feedback for developers to be able to understand whether the code they are
working on is releasable (i.e. passes all tests, security audits, etc.).

From a manual, multi-stage shipping processes, that involves one or multiple test environments, this typically means
automating much of the testing and security auditing and being able to run all of it in build pipelines (this has the
added effect of helping alleviate [The Test Data Problem](the-shared-test-data-problem.md)).

In an ideal shift-left process: All testing and security analysis (etc.) should be possible to run from a software
engineer's local IDE. Going further, it should warn the software engineer whilst they're writing the code (just like
how compiler errors and warnings show up in IDEs). These checks should be run in the official build processes anyway to
prevent code from being built or released that doesn't adhere to the requirements or pass tests, just in case the
software engineer didn't run the tests / security analysis locally.

Personally, I do NOT encourage any of the Release process to be shifted left into the Build process. Decoupling the
Build and Release process is a valuable boundary for semantics, security and to allow the two processes to be refined
individually for their unique purposes. By shifting Release activities into the Build process, we blur the line between
Build and Release, couple the two processes together and run the risk of opening security issues into the shipping
process (e.g. allowing unapproved artifacts be deployed to production). That being said, as the software engineering
sector matures we may identify aspects of the Release process that could actually be made as part of the Build process -
whilst I think this unlikely, we should be open to this possibility and change as needed.

## Security Testing

Further security testing, beyond the security scanning done in the [Build](#build) process, such
as [penetration testing](https://en.wikipedia.org/wiki/Penetration_test) can be extremely useful to detect security
problems with our systems built into our software, or in the infrastructure / platform that our software is built on.

If possible, some of this testing should be moved to run in the Code Analysis or Testing stage of the Build process, so
that it can be done earlier (see [shift-left](#shift-left)).

However, a lot of this testing requires the software to be running / live. Therefore, this testing must be performed on
the live production environment, if not on _all_ environments (production and non-production) - given the unfortunate
prevalence of production data ending up in non-production environments and those non-production environments not being
as secure as the production environment.

## Non-production environments

So far we've discussed the two main processes involved in the shipping of software, with each process almost entirely
automatable.

However, there are times when extra work is required that involves deploying artifacts to a non-production environment.

I am careful to use the term "non-production environment" as the term "test environment" is often used and I find using
it leads software engineers to drift down the path of producing permanent "test environments" used for post-build,
pre-release testing. This should be avoided, as it will
introduce [The Shared Test Data Problem](the-shared-test-data-problem.md).

I highly recommend _never_ putting production data into a non-production environment. This is not only a security /
privacy risk, but also often a sign that you may be treating a non-production environment as a test environment and are
attempting to incorrectly solve [The Shared Test Data Problem](the-shared-test-data-problem.md). Instead, seed data in
non-production environments, using specifications or seed functions.

However, there are still use cases for non-production environments:

- Ad hoc system-wide performance testing
    - Use production versions first to set a baseline (and identify bottlenecks). Then fix those bottlenecks and deploy
      development versions to test performance boost. Once happy that performance has improved for a single component,
      release it to production and set up a new baseline before moving on to the next bottleneck - this encourages
      continuous integration / deployment, reduces the likelihood of multiple changes causing production issues, and
      provides a structured and rigorous methodology to ensure that each change made a measurable improvement.
    - One or more components to test (e.g. integrated test isn't fun, but it's sometimes easier to quickly spin up an
      environment and pin down performance bottlenecks)
    - If frequent (not ad hoc) performance testing is required it should be built into the shipping process as part of
      regular testing, and done at a component level (not integrated).
- Demonstrations
    - If you're producing a service, you often want to showcase that service. Demonstration environments are production
      environments that contain only test data. These are subject to the test data problem if long-lived and used by
      multiple demonstrators, but since demonstrations usually (and ideally) have no impact on the ability to release
      software, it is an acceptable problem to deal with and easily mitigated by making fresh demonstration environments
      easy to recreate.
    - If demonstrations _do_ affect the ability to release due to some requirement of doing non-production User
      Acceptance Testing (UAT), then efforts should be made to remove this requirement, so that we can deploy to
      production as quickly as possible and get _real_ user feedback on new releases. If the requirement is unremovable,
      then time a release spends in the UAT / Demonstration environment, before it is promoted to Production, should be
      minimised as much as possible.
- 3rd party usage for their own testing/verification.
    - 3rd parties might not be as advanced in their testing as you and believe they can solve or work around the test
      data problem (they can't, but they believe they can), and so want a test environment to use to verify their
      changes. Sometimes this is legally or contractually required, and whilst it may seem frustrating, it's sometimes
      more diplomatic and easier to comply than to fight with 3rd parties. You can help mitigate the problems with the
      test data problem that 3rd parties have by resetting the environments test data regularly.
    - 3rd parties may also want to test YOU abide by your own specifications. Ideally, you'd coordinate with 3rd parties
      and do proper contract testing, but if that is not possible, then providing a test environment to a 3rd party
      where you provide a mechanism for them to configure an environment in different "states" for them to test that
      your produced API / service specifications match your environments behaviour (if for some reason they don't trust
      you). It's very important here to keep these environments and production as identical as possible.
