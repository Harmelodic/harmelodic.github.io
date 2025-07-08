# Effective error / exception handling

Error/Exception handling in programming is when we write code to "handle" what should happen when things go wrong as
part of processing something.

When developing any sort of application, "things going wrong" can be classified in the following ways:

1. An issue that occurs that *cannot* be recovered from in the application code.
2. An issue that occurs that *can* be recovered from in the application code.

In some programming languages, there is a neat (in my opinion) separation between "Errors" and "Exceptions": the
unrecoverable issues are called Errors and the recoverable ones are called Exceptions.

Errors are innately unrecoverable, and the application will either crash or be killed by the Operating System.
Exceptions *may* cause the application to fail processing something, but it probably shouldn't crash the application or
cause the Operating System to kill the application's process.

Since errors are innately unrecoverable and most of the time don't need "handling", I'll be talking about application
development, __the scope of this blog post is around *Exception* handling__.

## Why Exception Handle

Exception handling is all about safety, user experience and maintenance.

Safety: If an exception is thrown, then we want it to be handled in a controlled manner that doesn't result in
vulnerabilities (information being exposed, errors being used as an attack vector) or unexpected issues.

User Experience: If an exception is thrown, then we want the user to be informed of that in a controlled way, as well as
data being treated correctly (data being saved, rollbacks being performed, etc.).

Maintenance: As we develop software, we tend to split software into components & layers, and abstract implementation
details inside of classes, methods and functions. As we abstract the implementation details of how something is
processsed, it is preferable to also abstract what can go wrong, rather than have higher-level components deal with all
possible exceptions that come from our implementation. Whilst often this results in further exceptions being thrown,
more contextually appropriate exceptions hide the implementation details.

I've often heard developers discuss the questions: "Should we exception handle, or not? If we should, what's the best
way to do it?".

These question often arise from 2 areas:

1. A frustration with feeling required to do exception handling when it isn't needed. Writing the the "happy paths" of
   how to process things is relatively easy (and fun) to do, but writing all the code to handle when those things go
   wrong is often a lot more work and often a lot less fun, since often a lot more *can* go wrong.
2. A desire for an easy & effective way to do exception handling, when we believe it is required. Exception handling can
   often be quite involved, depending on the language, and many developers feel it clutters the code.

These are understandable sentiments, and so I'm going to throw my thoughts into the conversation.

Here's some bold statements:

- For most software that is written (not including hacks), developers must handle exceptions that occur, if it is to be
  safe and useful.
- APIs should make it clear what can go wrong and force consumers of the API to choose not to handle them, rather than
  choosing *to* handle them.
- There is no "one rule that fits all" when it comes to how to implement exception handling, it's more like "two ways of
  thinking and a few rules".

## Implementing Exception Handling

### Checked & Unchecked Exceptions

In languages such as Java, Exceptions are split are either:

- Checked Exceptions: exceptions that the compiler checks to see if they have been handle.
- Unchecked Exceptions: exceptions that the compiler does not check to see if they've been handled.

In Java, this results in the following class distinctions:

- `Error` - unrecoverable issues.
- `Exception` - Checked Exceptions
- `RuntimeException` - Unchecked Exceptions.

Personally, I think the name `RuntimeException` is a confusing one, as ALL Exceptions (and Errors) occur at runtime.
Therefore, I find it helpful to think of `RuntimeException`s as exceptions that are *handled* at runtime, rather that
being handled explicitly by developers in the code, thus at compile time.

So, why do we even have unchecked exceptions? If executing piece of code may result in an exception being thrown, why
not handle all of those cases? Wouldn't that be safer?

Yes, but that's not always the most valuable thing for a piece of software or to a developer. Dun dun duuuunnnnn!
Nuance?! Who knew?! Here are some common complaints I've heard/read from developers:

> Checked exception handling adds more code to an application's codebase. More code means more opportunities for bugs to
> occur inside of the code base.

I often find this is not the case when doing exception handling as exception handling is usually the code you write to
handle the class of bugs associated with "what happens when processing goes wrong".

> Checked exception handling adds more code to an application's codebase. More code means more code to read and
> understand before being able to contribute, edit and change code. This makes it harder to maintain, not easier.

This can be absolutely true, for small, simple and/or non-critical applications. However as the application grows and/or
the criticality of the application grows, the importance of handling exceptions carefully in the different components of
the application usually becomes highly valuable for maintainance and developer confidence in the application's
robustness.

The added coverage of handling exceptions being present in the code can also convey how careful and serious a developer
must be when performing changes to the application code.

Finally, effective exceptional handling where we abstract exceptions just as we abstract implementation details, should
result in changes to one part of an application not affecting all other parts of the application. This often makes
refactoring & maintenance easier. Ineffective exception handling absolutely makes maintenance harder. I think a lot of
developers believe exception handling is ineffective because they've only experienced ineffective exception handling.

> Checked exception handling adds more code to an application's codebase. More code for exception handling can just
> result in code for the sake of code and provides no extra aide to the developer or the consumer of the application.

Again, for small, simple and/or non-critical applications this can be absolutely true, and it can be much tidier in the
code base to let all exceptions bubble up and be handled at the top of the stack, because the number of exceptions are
small. As the number of exceptions increase, and as the desire for components to be more scoped on their own tasks than
needing to throw or handle an endless list of exceptions, I prefer to handle exceptions where they occur, even if that
means I'm often wrapping and throwing exceptions.

For application developers that prefer using an Inversion of Control (IoC) system (where unchecked exceptions are caught
by the IoC system and are handled according to instructions given to it by developers or handled in a default manner),
unchecked exceptions can also be a neat way of offloading the exception handling to a separate system. Personally, I've
not found this a better paradigm, as I value handling the exceptions in a controlled, explicit way where the the
exception occurs more valuable to me when developing & maintaining code than giving exception handling to a separate
system - this allows me to contribute to parts of a codebase without needing to understand the system as a whole, which
is very useful for larger or more complex systems.

> Sometimes the safest and quickest thing to do is to let a process/thread just crash by throwing an unchecked
> exception, rather than try to handle the exception and recover.

Absolutely true, the key word in this is "sometimes".

This complaint touches upon notions derived from [Fail-fast systems](https://en.wikipedia.org/wiki/Fail-fast_system).
However, I think an important to distinguish this into two parts:

1. Preventing exceptions
2. Handling an issue by throwing an unchecked exception

Fail-fast systems report issues with inputs or the current state of a system as earlier as possible to remove the number
of possible exceptions that can go wrong, rather than handle each of those exceptions. This is *preventing* exceptions
from happening, and should absolutely be done by implementing validation checks on inputs and verification checks on
system state. However, it will not be possible to completely prevent all exceptions this way, thus normal "try/catch"
exception handling will still apply.

In certain situations, we as developers may introduce a programming error into an application inadvertently, which has a
knock on effect of causing an exception. In these cases, these exceptions that are thrown are probably better handled as
unchecked exceptions, so that the rest of the application doesn't need to deal with a fault that lies with the code, not
with a runtime irregularity.

For example, when generating encryption keys in Java, we can instantiate a `KeyPairGenerator` as follows:

```java
KeyPairGenerator generator = KeyPairGenerator.getInstance("RSA");
```

The `KeyPairGenerator::getInstance` method throws a `NoSuchAlgorithmException` because "RSA" might not be a valid
algorithm. Given this is often just a hard-coded string in applications, this would produce the
`NoSuchAlgorithmException` if it was incorrectly set - which is an checked exception. In this case, I would attempt to
create the KeyPairGenerator at the start of the application, and if a incorrect algorithm was used, then just throw a
runtime exception and error out, as there is no point continuing with a broken KeyPairGenerator.

Incidentally, in this situation, I would prefer that the method took
a [enumeration](https://en.wikipedia.org/wiki/Enumeration) rather than a String and thus not need to throw this
exception as it would only compile if it had been given a valid enumeration - but since it doesn't, I'm using it as an
example.

To summarise Checked vs Unchecked Exceptions:

- Unchecked exceptions can be useful as they allow a developer to choose to ignore handling an exception within the
  context of the code and instead either (a) handle the exception elsewhere if desired or (b) treat the exception as if
  it were an Error.
- Checked exceptions force exception handling to be coded before compile time, which leads to a safer and (often, but
  not always) more maintainable codebase.
- Given these trade-offs, it's important to critically think about the context of the software and its components of the
  before choosing between handling an exception as a Checked or Unchecked exception. (Doing this more makes you faster
  and better at it, and it soon becomes natural)

Personally, I prefer to handle Checked Exceptions for most things and have the compiler enforce exception handling, then
selectively use Unchecked Exceptions for exceptions that I want to simply handle as if they were Errors.

Interestingly, Oracle has a small written piece
on [Unchecked vs Checked exceptions](https://docs.oracle.com/javase/tutorial/essential/exceptions/runtime.html), in the
Java Tutorials, that lines up with my opinion... mostly.

### Thinking about Contracts

In order for an exception to be handled, an exception must be thrown. Functions/methods throw exceptions.

Just as with the function name, arguments and return type, the exceptions the functions throw are part of the function's
definition or "contract". Upholding that contract to prevent breaking changes being introduced and causing problems is
an important part of code maintenance - especially for functions that are widely used (e.g. in libraries or core
components of systems).

Using the above analysis of Checked vs Unchecked, it can be further beneficial to the consumers of functions to be given
Checked exceptions as this provides a more explicit contract for what to expect the function to do. It is then up to the
Consumer to decide how they wish to handle the exception, if they can.

Some systems take a different approach, such as the Spring Framework for Java, which for... some reason, throws almost
exclusively unchecked exceptions. This has the nice effect of not needing to wrap exceptions as `RuntimeException`s when
you don't want to handle them, but has the negative effect of massively obscuring the contract for the Spring-provided
functions. Maybe this is intentional, but it often makes exception-handling with Spring awkward, as you're not 100%
confident that all the exceptions that the function might throw have been handled.

Some people [reason this](https://stackoverflow.com/questions/23178952/why-spring-handles-only-unchecked-exceptions)
choice to be because of "best practice" or reducing code clutter or following some design principles or increasing
decoupling, but most of this is plain false when you actually think about it, and I've found little to no evidence of
Spring's decision-making to use almost exclusively unchecked exceptions.

My suspicion is that they intend for Spring to handle all exceptions via some Inversion of Control or Aspect-Oriented
Programming paradigm - both of which result in exception handling being separated from the code that consumes methods
that throw exceptions - in my opinion, this reduces code cohesion and makes control flow unclear.

...or someone at Spring read "Clean Code" and took everything to heart.

### Catching and handling Exceptions

Assuming a checked exception has been thrown that we wish to handle, there are a multiple ways to implement handling the
exception:

First, there is the "catch, wrap and throw" method, which is very common. This is where we catch the Exception thrown,
wrap it in an exception more appropriate for the current layer/component to abstract the implementation and give context
to a stack trace that may be generated later, and then throw the exception to the consumer of the function.  
This methodology has benefits such as keeping exceptions thrown to consumers more contextually-relevant and decouples
the implementation details of the function from the function's contract. However, depending on how many exceptions are
thrown during execution, and how many are thrown - this can introduce quite a lot of code to simply wrap & throw
exceptions to the consumer of the method. This can be acceptable pain in a small context, but when it's occuring a lot
within a codebase, refactoring can be really beneficial to increase code-cohesion.

Another method of exception handling is to "catch, recover and continue". This is where we catch the exception thrown,
recover from the issue (by backing off to some default values, or retrying processes) and then continuing as normal.
This methodology is highly dependent on whether recoverability is possibly given the business requirements, but has a
neat benefit where we are explicitly trying to recover and continue the process, given it may be possible, to increase
the robustness of the process. The down-side is that the process is not failing fast, and we are spending our
development time improving robustness, when it could be easier for the consumer of the function (or the end user) to
just retry the process themselves, as they require.

The final method for exception handling is to "not catch, and only throw". This is where we implement zero exception
handling and instead just allow the exception to be passed to the consumer directly.  
This benefits from basically zero code additions (other than stating in our function's contract that we throw the
exception), but means that an implemention detail of our function is exposed to the consumer. Given this, I usually find
this method only useful when the exception I would throw to the consumer is the same as the one caught - therefore I can
save myself some lines of code and just throw the exception.

As we can see, there are different trade-offs for different methods, and it's up to the developers of the software to
decide how to handle each exception that is thrown. It sounds like a slow and nasty business, but gets quite easy and
natural the more you do it, and the result is safe, maintainable code that has a good user/consumer-experience.

### Can multiple return values or Optionals be good alternatives?

Some languages and developers don't want to handle exceptions in this "throw and catch" way, and instead return
something different.

The two main ways to do this is to either:

1. Return multiple values from a function (the caller receives a successful result or an error/exception result)
2. Return an Optional value, where a successful result may be present in the Optional.

In the case of multiple values, I view this as basically the same as "throw and catch", however we're simply "throwing"
the exception in a different way. Rather than catching the exception, we instead check if the error/exception result is
not null before do exception handling. Personally, I don't find this any better than a try/catch system - in fact, I
think it's slightly worse as I usually view return-values as indicative that everything went fine, and a try/catch
system is used for when something went wrong, which I find a neat separation of concerns.  
Of course, sometimes you don't get a choice. Languages like Rust and Go do not provide an exception handling system,
opting instead for handling errors in the return values. Languages like Java, JavaScript, C++ and Python provide a
try/catch exception handling system. My advice would be to use the system the language provides you (and not try to
force your preferences on a language system not designed for your preferences).

In the case of Optionals, I think Optionals should be used for a different purpose than exception handling. Optionals in
return values convey (to me) that a return value of `null` is an *acceptable* result where everything went right and the
result was `null`. This should not be confused with exception-handling since that deals with the business of how to
handle situations when things have gone wrong. Basically: Optionals aren't *for* when things go wrong, so don't use them
for that!

## Conclusion

Of course, some systems need to never fail, and every possibility should be known and factored out at compile time, such
as systems for Space-travel, medical systems and... basically anything that could result in life or death.

The less critical the application, or less maintenance it needs, or less we care about code cohesion, the more you can
fail fast and care less about exception handling.

If in doubt: Throw exceptions, handle exceptions, and be kind to your fellow developers & users. I take the view that
safer code usually leads to better software.

~ Harmelodic / Matt Smith
