# Organising Software Engineering Teams (2019)

> Originally published: 27 November 2019

Not interested in the Why but the What? - Read <a href="#the-structure">The Structure</a>.

## The Challenge

Our manual, auditable, securable, tangible, form-filling processes have been modernised, been "virtualised", been made "
digital"  - in other words, they've turned into software.

Some organisations figure out quite quickly that the world has changed and that instead of being
_**An Organisation that does X, Y & Z**_, they need to become
_**A Software Engineering Organisation that also does X, Y & Z, so they can pay the bills**_.

Which means that these organisations needs to write software, and to do that they need Sofware Engineers. However, once
you have engineers, you introduce a different problem:  
How to build, grow and manage your Software Engineering teams.

This is one of the key aspects to growing a modern organisation and has been one of the core elements behind companies
like Netflix, Spotify, LinkedIn, Facebook and Twitter sprinting ahead of the competition and having massive success. Not
only have they got a killer idea, but their Software Engineering structure and culture has led them to grow and adapt.

Every big organisation that I've ever dealt with has struggled with how to organise and run their Software Engineering
departments.  
Software Engineers are not like normal employees. The world of work has changed from being factory labourers doing
repeatable tasks and tired office workers filling in forms. Those tasks have been automated through robotics and
software.

Software Engineers aren't _Code Monkeys_ that you give a task to and then they go and do it (though a lot of Software
Engineers I know ARE _Code Monkeys_ and enjoy doing just that).

Software Engineers are diverse.  
They're creatives, designers, problem-solvers, academics, musicians, artists, foodies, cyclists, petrol-heads,
travellers, carers, fashionistas, and more.

They're human beings - with ideas to contribute and questions to ask.

Creating a structure for such a diverse group of people to work within to keep them focused, productive, supported and
happy is _really_ difficult.

I've been helping organisations do this for a while now. So here's the structure I would establish, in
2019.<sup><a href="#ref-1">[1]</a></sup>

## Existing Structures

SAFe, LeSS, DaD, Lean, Agile, Lean-Agile.  
Wherever you look, there are articles and books on structure that people have devised that invents roles, puts
questionable limits on people and makes decisions for you.

Don't get me wrong, some of these structures have potential and provide benefits to organisations - Hell, even this post
is adding to that cacophony! - but the point of these modern ideas and ways of working is to be _flexible_ and
_responsive to change_. Going 'by-the-book' ain't that.

## Too Many Cooks

Why do we need ALL of the following?:

- A Programme Lead
- Release Train Engineers
- Solution Train Engineers
- Engagement Manager(s)
- A Principal Technical Architect
- An Architecture Team
- Project Managers
- Product Owners
- SCRUM Masters
- Business Analysts
- Dev Team Leads
- Dev Teams
- A Platform Team Lead
- A Platform Team
- A UAT Test Team Lead
- A UAT Test Team
- A Performance Test Team Lead
- A Performance Test Team
- A SIT Test Team Lead
- A SIT Test Team
- A Penetration Test Team Lead
- A Penetration Test Team
- and so on...

I'm exaggerating a bit with this list, but my fundamental point behind this list is:  
_We have too many Roles in organisations to facillitate the job of "making software"._

A lot of these roles can be combined, some can be automated, and some can be completely removed.

## The Structure

Working with an example is best, I'll try to be as abstract as I can.  
Let's assume you're trying to architect, develop, deploy and maintain **a very large software SOLUTION**, involving *
*many SYSTEMS**.

### System-level Development

A _Team_ manages a single system. With the team made up of:

- A Team Lead
- 5-9 Team Members

A _Team_ is responsible for all aspects of their System and their _Team_, including:

- Requirements Gathering & Analysis
- Architecture for their System
- UX/UI Design
- Application Development
- Platform Engineering
- Testing
- Automation & Delivery
- Support & Maintenance
- Self-management of the _Team_
- Organising fun activities for the _Team_

I use the term _Team Members_ purposefully here.  
As the responsibilities of _Team_ are varied, it is naive to expect that all skills and capability is distributed evenly
across the _Team_. Some _Team Members_ will be better at the more _BA_-esque roles involving Requirements Gathering &
Analysis, other's will be experts in Application Development, other's in Platform Engieering and Architecture.  
The core goal of any _Team_ is to share knowledge and empower each other, so that if one _Team Member_ is off due to
sickness or leave, the rest of the _Team_ could collectively fill in for the missing _Team Member_ until their return.

This provides:

- _Team Ownership_ of their System, thus removing the need for a Product Owner.
- A _DevOps_ culture/implementation where the Team is responsible for all aspects of Application Development (Dev) and
  Platform Engineering, Automation & Delivery (Ops) for their System.

The number of _Team Members_ depends on the size and complexity of the System, but overly complex Systems should be
broken down into less complicated or even simple Systems and then developed by multiple _Teams_. 5-9 _Team Members_ has
been a good fit in the past that allowed creative discussions to flow (
avoiding [HiPPO](https://whatis.techtarget.com/definition/HiPPOs-highest-paid-persons-opinions)
and [Groupthink](https://en.wikipedia.org/wiki/Groupthink)) whilst still having enough people in the team to get the
work done quickly.  
If in doubt, use the [2-pizza rule](https://www.google.com/search?q=2%20pizza%20rule%20team%20size).

The _Team Lead_ is the role of an experienced member of the team who has the responsibility of making final decisions,
ending arguments, providing an experienced expert opinion and generally enables the team to do their job. They can also
be the SCRUM Master if using SCRUM.  
The _Team Lead_ also functions partly as a Project Manager by engaging the _Solution Administration Team_ (detailed
below) to facillitate recruitment into the _Team_ and any other business-related matters regarding their _Team._  
Overall, a _Team Lead_ should not only have a large technical background, covering everything from Development to
Architecture, but also have all the soft skills and leadership ability to run a successful _Team._  
I would recommend, to prevent a _Team Lead_ from becoming too content in their role, that 20% of their work-time be
spent on researching the best practices and newest innovations, so the _Team_ can stay ahead of the curve.

For most cases, when I've been a _Team Lead_ in the past, I've found running the team using SCRUM (i.e. sprints,
standups, retrospectives, etc.) has been the most appropriate. However, it can depend on the context and systems like
KANBAN can work better; For example, if the team does more ad-hoc consultancy than developing a specific _System_.

### Solution-level Development

The only _Solution_-level development that needs to be thought about is:

- Architecture of the high-level _Solution_ design.
- How each _Team_ contributes and fits together as part of that high-level _Solution_ design.

This is usually done by: An _Architectural Team_, and in this Structure:  
**An _Architectural Team_ is made up of all _Team Leads_ from each _Team_.**

Having a centralised _Architectural Team_, away from normal _Teams_, that make decisions about how the _Solution_ is
built and what technologies should be used, is a _**terrible**_ idea.  
Not only does it mean technical decisions are made by the people who aren't dealing with the techincalities, but it
means that new Architectural ideas being innovated in _Teams_ are not recognised and shared amongst the rest _Teams_
working on the _Solution_.  
Put simply, centralised Architectural Teams have nearly always turned into a circle-jerk of supposedly experienced
engineers, who haven't touched code in years (or who never have) making grand, "big picture" decisions and pushing them
_down_ onto _Teams_.

Contrary to that, an _Architectural Team_ made up of all _Team Leads_ from each _Team_, provides all the benefits of a
centralised Architectural Team (i.e. high-level overview and design of the entire _Solution_) without any of the "
circle-jerk"-ing cons, due to the fact that _Team Leads_ recognise new innovations from _Teams_, are highly experienced,
and will have to deal with the consequences of bad _Solution_-level Architectural decisions and so will have an active
mandate to ensure the correct decisions are made in the right way.

### Dedicated Testing

ALL testing should be automated. However, there is sometimes a need for a _Dedicated Testing Team_ with a particular use
case: _Solution_-level Penetration Testing.

All other use cases, including unit tests, component tests, integration tests, UAT tests and performance tests can have
those tests written by a _Team_ for the System they are developing and then automated as part of CI/CD pipelines.

The amount of testing required here may also require a few extra Testers dedicated to writing these tests (such as
_Solution_-level Integration, Performance and UAT Testing), however once written they can be developed on and maintained
by the Teams, and run via regular automation jobs.

### Inter-Team Communication & Alignment

Given that _Team Leads_ make up the _Architectural Team_, this team responsibilities can be expanded to communicating
_Blockers_ to one other in regular _Solution_-level architectural meetings. This way, _Blockers_ can be brought up
shortly after they appear (or beforehand if there is a known upcoming _Blocker_ that needs to be prioritised) and the
_Team_ that is responsible for resolving that _Blocker_ can re-prioritise their backlog to ensure that the _Blocker_ is
resolved promptly. This keeps _Teams_ in alignment and not continually blocked by one another.

To ensure knowledge and experience is shared amongst the _Teams_, regular _Demo Days_ should be organised to provide
_Teams_ or specific _Team Members_ the platform to talk about innovations they've developed, problems they've come
across and then resolved, new tools they've found useful, and so on.  
Generally, it's an opportunity to share knowledge and be inspired to make improvements and changes in your own _Team_.  
_Demo Days_ also give the _Team Leads_ the opportunity to provide an update around the _Solution_-level architecture and
any changes, updates they have come up with and if anyone attending the _Demo Day_ have any questions, queries, worries
or woes about the future of the _Solution_.

_Innersource_ is one of the most game-changing things to implement in any Software Engineering organisation. If you look
at internet as it is at the moment, on of the big reasons why so many organisations have been successful is because of
the HUGE range of open-source tools that are available to them to use on their own solutions. A strong _Innersource_
community culture effectively replicates the same impact that open-source has had on society, but within your
organisation.  
With more tools and products available to choose from and contribute to, a strong _Innersource_ community has been one
of biggest catalysts for evolution in Software Engineering practices and development.

Finally, active communication between _Teams_ is also beneficial. Nowadays, this has taken the form of everyone working
on the _Solution_ being on an Instant Messaging System (IMS) such as Slack, MS Teams, RocketChat, or Keybase, and then
communicating with one another on that IMS as they see fit.  
One restriction I have found good to implement, is to ensure that _Blockers_ are not reported and dealt with immediately
via _Team Members_ talking on an IMS. Minor issues being reported and dealt with are fine, but as a general rule:
Anything that takes 30+ minutes or more should be reported via the proper channels to ensure that the _Blocker_ is
tracked and dealt with appropriately. All this ensures that _Team Members_ are focused on the work that has been planned
and are not constantly fixing problems reported via an IMS.

### Solution Administration

Depending on the size of the _Solution_, a certain number of administrative staff need to be in place to manage:

- Recruitment for Teams
- _System_ & _Solution_ estimations (pulled for ticket-tracking software)
- Structure Coaches

Recruitment is important, of course. Engineers leave organisations for a variety of reasons and it's important that
there are people in place to sort out hiring the right people for the right role in the right team.

_System_ & _Solution_ estimates have always traditionally been a responsibility of the Project Manager. However, with
the rise in ticket-tracking software, like JIRA, and the expansion of tools & plugins that generate estimations and
reports from JIRA Projects, has meant that the hardwork that Project Managers used to do around estimations, project
trajectories and reporting, can now be generated by the click of a button on JIRA.  
That being said, the software isn't perfect and upper-management that require these sorts of reports and estimations for
financial and general business reasons aren't completed catered for by the reports that the software generates.  
As such, there is still a need for human element to generate the specific reports that upper-management require and to
provide a human interface to upper management if they want to engage further down to the _Solution_-level.

All of this structural, Agile mess needs people who know how this works and know what makes _Teams_ successful. That's
the role of the Structure Coaches. These are people who will facillitate the implementation of The Structure as well as
engage with _Teams_ to ensure they are reflecting, improving and evolving their Ways of Working - where they aren't the
Structure Coach will help the _Team_ make that happen. They also would organise things like _Demo Days_ and engage with
the rest of the business to ensure that the external tools and systems that _Teams_ need are made available to the
_Teams_.  
Structure coaches need to be energetic, knowledgable in how to manage people, have enough technical knowledge to
understand the needs of _Teams_ and outspoken enough to challenge people.

---

Enjoy changing the world!

Harm x

---

<sup id="ref-1">[1]</sup>  
"The only constant in life is change" ~ Heraclitus.  
Post-2019, there will be new organisational problems to solve and this structure will need to evolve and change.
