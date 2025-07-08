# Principles for managing npm packages

> Originally published: 26 November 2018

## Why do we need them?

When developing a frontend or a Node.js application, it is highly likely (and recommended) that developers
use [npm](https://www.npmjs.com) to manage their dependencies.

However, npm has a lot of...risk.

Unlike communities around other languages where established frameworks, libraries and best practices have been put into
place and a project's dependency tree is relatively small, the JavaScript community has been quite the opposite.

Since around 2009/2010 (when Node.js & npm were launched), including lots packages in JavaScript projects has become
increasingly popular, given how easy it is to write JavaScript and how easy npm makes it to publish packages for public
use.

Some advantages to this have been:

- An increase in code sharing and reuse.
- Public access to libraries & frameworks written by more skilled/knowledgable developers.

However, there are plenty of disadvantages:

- An increase in instabilty of projects using unstable or out of date packages.
- Packages being used by unknown, untrusted individuals.
- Inclusion of code that may not be tested or follow established standards/patterns.
- Large amounts of redundant code included in artifact/bundle.
- Risk of unsecure, malicious packages being included and hidden in a large dependency tree.

These instablity and security concerns have been publicly voiced and:

- [How one developer just broke Node, Babel and thousands of projects in 11 lines of JavaScript](https://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/)

- [I’m harvesting credit card numbers and passwords from your site. Here’s how.](https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5)

- [A package owner transferred ownership of a relatively popular package to a hacker](https://github.com/dominictarr/event-stream/issues/116)

**We need principles to follow.**

I've written some:

## The Principles

1. Only use mature packages.  
   (i.e. has clear, easily discoverable documentation and has at least one stable major version - e.g. `1.0.0`).

2. Don't install **_large_** packages when you're only going to use a single piece of functionality from it.  
   If you need more functionality later, you can refactor.

3. Don't blindly follow trends, only use packages you need.

4. When you're about to install a new package, ask yourself the question:  
   _How long would it take me to write and test the functionality I need out of this package?_

	- If the answer is: _Not very long_, then don't install the package and just write it yourself.
	- If the answer is: _A while_, then consider writing it yourself, if you have time.
	- If the answer is: _A long time_, then it's likely you will require it (or a similar package).

5. Ensure packages are maintained.  
   (Rule of thumb: Has had a release in the past 2 months).

6. Ensure packages are well-tested.  
   (Ideally, have a test coverage above 75%)

7. Implement package testing in your testing CI/CD pipeline.
   These tests should include:
	- The build doesn't break with new & upgraded packages.
	- Passing an `npm audit` (Ideally should find 0 vulnerabilities).
	- Complies with all of the above principles.

## The Why

1. Immature packages carry with them:
	- Instability.
	- Unknown security vulnerabilities.
	- Lack of support/documentation to refer to when attempting to fix issues.

2. Including large packages for small pieces of functionality introduces a lot of redundant code into your final
   bundle/artifact.  
   Code-splitting can remedy this partially. However, engineers should be skilled enough to write single pieces of
   functionality when needed.  
   If they are not, then this provides a good learning opportunity for them (or the task can be handed off to a more
   experienced developer).

3. Engineers should think about what is required when developing a solution.  
   Following trends for the sake of following trends can be rewarding from a marketting perspective, but can introduce
   inefficiencies, new unsolved problems and messy code.

4. Engineers should actively avoid situations like
   the [the left-pad incident](https://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/).
   If a piece of code can be written easily and quickly, then there is little harm in writing, for the sake of stability
   and security.
   Implementing an extreme level of code-reuse can lead to a volatile project, where you're attempting to manage
   packages (things out of your control) more than you are managing your own code (things in your control).

5. Unmaintained packages lead to:
	- Code being included in your bundle that could eventually contain security flaws that are never fixed.
	- Locking your project into a unsustainable architecture that is difficult to refactor out of.
	- Incidents similar to [the event-stream issue](https://github.com/dominictarr/event-stream/issues/116)

6. No one knows whether something works unless tests have been written for it.

7. Continuously testing your dependencies for stability & security issues leads to a higher likelihood of security &
   stability in your project.
