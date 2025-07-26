# Automated Code Refactoring

Automated code refactoring is the process of refactoring code in a more automated way, rather than purely manually.

Manual refactoring involves a software engineer editing the code to improve it (e.g. performing maintenance, bumping
versions, adjusting code style, replacing implementations, etc.).

Automated code refactoring involves scripts and other software tools executing over codebases to do the same or similar
refactorings faster and can therefore be made applicable to more codebases - e.g. multiple code repositories.

## Adopting the approach

In its most raw / manual form, this involves a software engineer taking multiple codebases, and executing a script or
software tool that automatically adjusts the code in all the codebases appropriately, before committing and pushing
the changes.

> Since most engineers use Git, these changes are usually created as Pull Requests or PRs (rather than be directly
> applied to a [trunk](https://trunkbaseddevelopment.com/)) and so have come to sometimes be known as "PR campaigns" -
> as a slight reference/pun on the same informal term for an advertising campaign.

The kinds of refactoring typically done at earlier stages of automated code refactoring are things like code formatting,
version bumping dependencies and simple migrations of code that uses deprecated components to one that uses modern
components.

Further automated code refactorings can take the form of more complex migrations. This has sometimes been given the
grandiose title of "Fleet Management", as a reference to the management of a fleet of vehicles.

As automated code refactoring becomes more prevalent in an organisation, the usage
of [standards](standards-vs-abstractions.md) and default abstraction behaviour becomes more valuable, as it makes
automated code refactoring even easier to perform.

## Tools

Most languages already come with linting and formatting tools to aid in defining and automating the formatting of code
to be consistent (e.g. `prettier`, `EditorConfig`, `go fmt`, `terraform fmt`, etc.)

[Renovate](https://github.com/renovatebot/renovate) (by Mend) and [Dependabot](https://github.com/dependabot) (by
GitHub) are two bot tools that automatically detect and update dependencies.

[OpenRewrite](https://docs.openrewrite.org/) (by Moderne) is a more fully-fledged automated refactoring system, already
used Spring to provide "recipes" (scripts) for performing refactors when upgrading Spring Framework versions. 
