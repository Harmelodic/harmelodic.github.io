# Introduction

I'm starting a new category of blog posts that I'm calling "Build & Deploy".

Each post will be focusing on a different "project type" (Java web service, Frontend webapp, Terraform infrastructure,
Java Library, etc.) and how to build and/or deploy that project type.

I won't be covering *every* way of building & deploying these project types. Instead, I'll be using my experience to
make opinionated decisions on how I think developers *should* be building, deploying and running these projects. I'll
endeavour to give reasons as to those decisions to provide clarity, but I'll be keeping them relatively succinct in
order to stay focused on the "Build & Deploy" stuff (expanded reasoning can be done in a separate post, if there's
enough interest from me & readers).

I also will be, most likely, focusing on the "Build & Deploy" processes of web-based technologies using the following
languages (Java, JavaScript/TypeScript/ECMAScript, Terraform, Go, since that's my expertise.

I will also, often, only be focusing on the *__either__* building *__or__* deploying - since they often separate
processes. Many project types get built into the same artifact (e.g. a container image), which then means the deployment
process is the same for all those project types. Rather than waste my time repeating myself, I'll instead be writing
separate blog posts and linking them.

## Build & Deploy (Generic)

From a very high-level point of view, the build & deployment process of software works as follows:

Building software is the process of taking code and creating deployable artifact. This is involves multiple steps:

- Code linting / scanning
- Compiling
- Running Tests
- Packaging
- Packaging again (sometimes)
- Publishing<sup>1</sup>

Deploying software is the process of taking a deployable artifact and "deploying" it to where it needs to
be [executed](https://en.wikipedia.org/wiki/Execution_(computing)). This also involves multiple steps:

- Defining deployment configuration
- Pulling / pushing the deployable artifact to a machine for execution
- Initiating the execution
- Enabling usage/rollout of features, post-execution.<sup>2</sup>

and that's pretty much it!

<sup>1</sup> *Depending on the project type, "Publishing" could be part of the Deployment process, but for most cases
I'd strongly argue that publishing a deployable artifact is part of the build process*.

<sup>2</sup> *Strictly speaking, "Enabling usage/rollout of features, post-execution." could be an entirely separate
process called "Releasing" but that's a bit more advanced and often unnecessary*

~ Harmelodic
