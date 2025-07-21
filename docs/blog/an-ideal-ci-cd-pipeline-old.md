# An ideal CI/CD pipeline (Old)

> Originally published: 27 November 2018

## Not applicable anymore

Like with some of my other posts - this really isn't applicable anymore. I think on reflection I was documenting what I
knew at the time, rather than critically thinking about what testing was needed and how to do it.

Nowadays, I shift tests left a lot and have a strong recommendation *against* creating permanent or automated
environments for testing.

I'll leave this post here, and maybe I'll write a new one on testing.

## Original Post

CI/CD pipelines have become an industry standard for taking a project from source code to a deployed package on a
platform.

As part of these pipelines, we need to perform different types of testing and analysis to prove that the code that has
been written complies with the standards that the software engineers define.

Here is my ideal pipeline that I work towards:

## Pipeline

- **Unit Tests** - tests the functionality for individual functions/methods within your application.  
  Requirement: A testing framework.

- **Component Tests** - performs component-level integration tests to test application functionality.  
  Requirement: An integration testing framework as well as mocks/stubs for an external services (Databases, APIs, I/O,
  etc.)

- **Code Analysis** - perform static code quality analysis on your source code. The failure threshold can and should be
  configured on a project by project bases.  
  Requirement: A static analysis tool(s) to do this, like SonarQube.

- **Security Analysis** - performs security and malware checks on your source code. Like with Code Analysis, the failure
  threshold can and should be configured.  
  Requirement: A security analyis tool to do this, like Snyk or JFrog Xray.

- **Build** - builds your application into an artifact and store it in a Repository Manager/Registry.  
  Requirement: A build/packaging tool, like npm or Maven, plus any software/plugins required for artifact, like Docker
  or the Fabric8 plugin.

- **Deploy to SIT** - automatically deploys to your first testing environment for System Integration Testing.  
  Requirement: Has passed all automated tests.

- **Integration Tests** - performs end-to-end integration tests on the entire system to ensure the application hasn't
  broken existing functionality and proves new functionality.  
  Requirement: End-to-end integration test suite.

- **Simple Performance Load Tests** - performs a short performance load test to prove application can handle high
  load.  
  Requirement: A performance testing tool, like JMeter.

- **Deploy to UAT Environment** - automatically deploys to second testing environment for User Acceptance Testing.  
  Requirement: Has passed all SIT tests.

- **Manual UAT Testing** - User Acceptance Testing performed by the client (or similar authoritative entity) to prove
  system meets requirements & expectations.  
  Requirement: UAT Test Team with appropriate resources & tools.

- **Deploy to Pre-prod Environment** - manual deployment to Pre-prod Environemnt for final testing before production.  
  Requirement: Has passed all UAT tests.

- **Penetration Testing** - manual penetration testing performed by a security/ethical hacking team.  
  Requirement: Penetration Test Team with appropriate resources & tools.

- **Deploy to Production** - manual deployment to Production.  
  Requirement: Has passed all Pre-prod tests.

## Pull Request Pipelines?

I have purposefully left out things like Merge Requests/Code Reviews as these should happen before this pipeline is
enacted.

However, when a Pull Request is created, then a simplified Pull Request-focused pipeline should be run.  
I would suggest that this simplified pipeline would run from _Unit Tests_ up to and including _Build_, however the build
should be tagged as a temporary build and stored in a separate location in the Repository Manager, to prevent confusion
and misconfiguration.

After being built, the application could be automatically deployed into an isolated, temporary testing environment for
the Pull Request Reviewers to check that the PR functions as expected.

## Overkill?

Many will view this pipeline as "overkill", and for most solutions, it is. This post, however, is not a "you must
implement this pipeline", it is a proposed goal that all engineers should strive for. I do not expect all engineering
teams in the world to abide by this, or follow it to the letter, as limitations (such as time and cost) can mean that
only part of this pipeline could ever exist for a particular application/system.  
For example, _Penetration Testing_ and _Perfomance Testing_ could be done periodically, instead of for every change, or
_Performance Testing_ could be done in the Pre-prod environment instead of the SIT environment.

This is simply my view of an ideal pipeline, based on my experience.
