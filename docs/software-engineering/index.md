# Software Engineering Overview

## 🚧 This is a work in progress!

This is a new section of my site that is still under-construction / not "finished".  
You can explore my work in progress, or instead view my [current posts on Software Engineering](../blog/index.md), or
come back later.

---

A (sort of) Software Engineering handbook.

## Contents

- **Philosophy and Practices** - A section for general software engineering values, principles and practices, covering:
	- Architecture
	- Contracts & APIs
	- Development
	- Operations
	- Organisational Operations (Management, Ways of working, etc.)

Documentation & guidance for different engineering ecosystems:

- **Platform**
- **Backend**
- **Web Frontend**
- **Data Science**
- **Mobile**
- **Desktop**
- **Embedded**

Each ecosystem covers:

- Specific principles & practices (inc. architecture)
- Per language covered:
	- Development specifics
		- Overview of the language ecosystem
		- Local machine setup
		- Shipping (Build & Release)
		- Doing specific things in that language
	- Operational specifics

Use to use:

- Java = SDKMAN
- Python = pyenv
- Terraform = tfenv
- Node.js = nvm

Now I use [mise-en-place](https://mise.jdx.dev).

## Purpose of these docs

- Mostly for myself as a reference, but if it's useful for others then that's good!
- A place to write software engineering thoughts, that isn't a chronologically linear "blog", but something more
  structured, and focused on different spaces.
- A place to write down my [software engineering philosophy](./philosophy-and-practices/index.md) and exercise my brain
  in envisioning "how things should be".
- Showcase my skill-set / experience / philosophy for future work opportunities.

## Developing your own handbook

- A useful way to document how engineering works in your organisation.
- Ecosystems: Documents how you expect ecosystems to be working (can help codify standards and conventions that you
  have in your organisation). Encourages sharing and discourages team-scoped docs describe their learnings.
- Philosophy: Treat it as an exercise for codifying what your engineering culture is (or what it should be). Talk
  about practices you like, to discover values and principles you adhere to (or want to adhere to), and write them
  down. Figure out the processes for decision-making and document them, to make them transparent and work for your
  organisation.
- Team practices (meeting cadences, on-call schedules, team-specific development practices) should be in the
  handbook, so that teams and see how other teams function, and can learn from one another via the handbook.
- Experiments trying things (new development practices, new library) should be documented in the relevant section.
- Documentation for systems should be with the system code (e.g. in the `docs` directory in a repo) and not in the
  handbook.
