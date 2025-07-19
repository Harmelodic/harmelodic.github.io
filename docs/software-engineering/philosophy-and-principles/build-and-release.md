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
- **Packaging** - Producing an artifact:
	- Bundling to create the artifact.
	- Storage of the artifact.

Example artifacts could be:

- Executables (e.g. Programs, Applications, Container Images)
- Code libraries for use in other projects.
- Deployment Manifests (e.g. Helm Charts)
- Infrastructure execution plans (e.g. Terraform plans)
- Specifications / Contracts (e.g. PACT contracts)
- Rules / Policies for use in analysing or linting other code.
- Generated code for documentation (or anything else)

Artifacts can sometimes be ephemeral (like Infrastructure execution plans), but most artifacts are permanent and should
be stored in some kind of Artifact Repository or Registry (different providers of Artifact Repository solutions call
them different things).

When storing artifacts, it is recommended to make artifacts immutable and not possible to overwrite - this prevents a
lot of Supply Chain Security issues.

## Release

Release processes take artifacts and make them available to users - in whatever way is relevant for that.

For executable services, this .... to be continued.