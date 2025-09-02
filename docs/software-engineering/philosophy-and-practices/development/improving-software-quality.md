# Improving Software Quality

"Cheap, fast, good". Choose two... Well, let's start by picking good, and then argue about cheap and fast.

Improving software can be a hard business, but now that it's 2025, it doesn't _have_ to be.

There are two sides to improving software:

1. Building better or more features.
2. Improve all the rest of the qualities of the software (often dubbed "non-functional" qualities)

It's worth noting that it's not only the _actual_ software that can be improved using the following principles &
practices, but everything "around" the software too (documentation, specifications, infrastructure code, automation
pipelines, metadata code, build files, etc.).

## When to improve

Improve the software when you need to improve the software.

If you don't need to improve a specific quality of the software, then don't improve it! Save your money and time and go
work on something else.

If you don't know if you need to improve the software, then dig into the "known unknowns" and see what you find, or
gather feedback from the users of your software - something will turn up!

## Improving features

Improving features should be done in two ways:

1. Ask for feedback from your users on what features they want improving.
2. Come up with your own ideas (innovate), and then see if the users want it (gather feedback).

So really... it's all about gathering feedback, just sometimes your ideas might spark that feedback.

## Improving everything else

To improve the quality of everything else (like reliability, security, code quality, compatibility, design,
accessibility, etc.) it helps to have any and/or all the following:

- Experienced people who know (a) what bad looks like and (b) how to turn bad into good.
- A good decision-making process in order for good decisions to be made quickly.
- Automated build validation & verification that blocks bad software from being built.
- Automated maintenance systems that passively & continuously updates (or suggests updates) the software.

The automated detection, code analysis checks, fixing and updating can be made substantially easier to perform
through [standardisation](./standards-vs-abstractions.md).

### Automated build validation & verification

Basically the stuff in the [build process](./shipping-software.md#build). Things like:

- Format checks
- Linting checks
- Spotting bugs
- Modernisation checks
- Code rules
- Tests

Of carrots and stick, this is quite "stick"-focused, as breaches / errors / infractions block the software from being
built.

### Automated maintenance systems

Passively continuous code analysis tools.

This can be as simple as giving engineers information for them to manually improve their software (SonarQube, etc.).

However, the better ones continuously improve the software automatically (or suggest to do so). This is stuff like:

- Dependency update bots (Renovate, Dependabot, etc.)
- Auto-formatters
- Systems detecting out of date code and running (OpenRewrite) recipes to improve the code.
