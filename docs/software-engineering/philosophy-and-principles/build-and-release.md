# Build and Release

Build is the word that covers the process of turning "code" into an "artifact".

Release is the word that covers the process of making that "artifact" available for use to users.

This stuff often comes under other terms like Continuous Integration and Continuous Deployment (CI/CD), or the Software
Supply Chain.

## Build

Build processes take code and produce artifacts. What the code is intended for, you might expect a different artifact.

In a build process you tend to want to do:

- **Code Analysis**
	- Linting to ensure adherence to a code formatting / style guide / code structure rules.
	- Make suggestions for modernising code.
	- Detecting bugs and code smells
	- Detecting leaked secrets
- **Compiling** - Turning the code into binaries, or other kinds of executable/loadable code.
- **Testing** - Running tests to give us confident the code is fit for purpose and bug-free. Can include:
	- Unit tests
	- Integration Tests
	- Integrated Tests
	- Contract Tests
- **Packaging**
	- Bundling to create an artifact.
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
development/pre-release/release-candidate artifacts.

## Release

Release processes take artifacts and make them available to users.

Release processes generally always begin with the publishing of a built artifact to an artifact repository that is used
for storing releases. When publishing release artifacts, it is recommended to make artifacts immutable and not possible
to overwrite - this prevents a lot of Supply Chain Security issues.

Beyond publishing an artifact, release processes can look quite different depending on what it is your releasing.
This tends to come in 3 forms:

- Releasing new versions of a service that users access remotely (e.g. APIs or websites).
- Releasing new versions of an application that users use locally (e.g. Desktop application or CLI program).
- Releasing new versions of the artifact publicly.

Let's see what each one looks like: ... TBC
