# The Shared Test Data Problem

> TL;DR: Using permanent test environments as part of your process for shipping software introduces an unsolvable
> problem that I call "The Test Data Problem".
>
> The only way to resolve the problem is NOT use test environments and to instead trust specifications / contracts.
> Trusting specifications can only be reliable and give you confidence if you do contract testing.
>
> Other non-production environments can still be useful, but for specific purposes, and should be direct reflections of
> the production environment.

## How the problem occurs

- Wanting to test a component of a system that integrates with other components of the system.
- Lack of confidence in specifications vs actual system behaviour
- Create an environment for testing these integrations.
- Run tests, and it works. So, let's have everyone do this!
- When everyone does this:
	- Other people are running tests, which mess up my tests because they mess with my test data.
	- Attempt to provide a system to create isolated test data for different components & tests (large coordination
	  effort to avoid test data collisions and even then collisions are sometimes unavoidable)
	- Even if your tests and test data succeeds, you don't feel confident and instead feel like your tests were able to
	  "thread the needle" as it were.
	- For those who do feel confident: you just tested your system against other test versions of other systems, meaning
	  you have little evidence to show that it would work in an actual production environment.

## The Shared Test Data Problem is unsolvable

When you introduce a scenario where you test multiple systems in parallel using a shared test data / context, testing
becomes fragile and introduces unsolvable problems primarily associated with test data.

Only by changing the axioms of how the tests are run can we return to safe, reliable tests:

- By not running tests in parallel, we can test each change sequentially and release safely. This usually results in
  extremely slow testing.
- By not testing multiple components and testing them individually, you remove the shared test data / context and
  fragility. This means we have to rely on other components behaving as expected by trusting their specifications.

## The solution is trusting specifications

Despite their being two solutions, in most situations only one solution is tolerable: Removing shared test data and
instead trust specifications. This is why I call the problem the "Shared Test Data Problem" because it firmly identifies
the main issue being the shared test data, and prompts engineers to disregard shared test data as being part of any
solution.

- Fundamentally, we shouldn't test our system depending on the correctness of another system.
- If the specs are correct, then there is nothing to test in the integration.
- So trust your specifications are correct
- Trust but verify
- Verify the specifications are correct by doing contract testing.

## Useful non-production environments

Just because using non-production environments for testing introduces the Shared Test Data Problem, doesn't mean that
non-production environments themselves are bad.

This is discussed more in [Shipping Software](shipping-software.md)
