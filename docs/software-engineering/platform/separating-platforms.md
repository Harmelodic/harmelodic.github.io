# Separating Platforms

## Context

In Platform Engineering, a platform has the following aspects:

- Some infrastructure for hosting software (applications, databases, blob storage, data warehouses, etc.)
- Some software to handle Identity and Access Management (IAM)
- Environments used for development, testing, operations and demos of software (there should be a minimum of 1 per
  platform: The Production environment)
	- Note: Environments are platform-specific.
- Software Engineers / IT staff using the platform to host software.
- End users of the hosted software.

In order to adhere to the [Separation of Concerns](https://en.wikipedia.org/wiki/Separation_of_concerns) principle (and
thus make software easier to manage, maintain, operate, etc.), I have found that it is best to have multiple platforms
for specific purposes, where the purpose is usually defined by whom the End Users are and what service we expect to
provide them. This means, in order to know what kind of platform to build, we need to know how organisations operate.

In an Organisation where Software is being built, you tend to have the following identifiable groups of people:

- Software Engineering / IT staff (i.e. the people who build & run the software)
- Operational staff (i.e. the people who actually make the business tick - i.e. the business' Domain Experts)
- Administrative staff (i.e. the people who organise, administrate and manage the other employees)
- Customers / Users (i.e. the people who the organisation provides goods & services to, in exchange for money).

On purpose, there should be defined services offered to End Users by the software hosted on the platform. This defines
the actual needs of the software that the platform will host, which effectively set requirements on what capabilities
the platform needs to provide, but also defines what [kind of Domain](https://github.com/ddd-crew/core-domain-charts) it
is (Generic, Supporting, Core) and so helps you plan for how much to invest in the software for that platform, and the
platform itself.

## Overview

Semantically 5 different platforms should exist:

- Staff IT Platform
- Business Management Platform
- Business Operations Platform
- General Data Platform
- Internal Development Platform

Whilst I believe it is valuable to keep these platforms separate, especially for medium-large organisations, many
organisations will merge these platforms into:

- General Internal IT Platform, that fulfils the purposes of all the following:
	- Staff IT Platform
	- Business Management Platform
	- Internal Development Platform
	- General Data Platform (sometimes or parts of it)
- Business Operations Platform
	- plus General Data Platform (sometimes or parts of it)

Some business may even merge these platforms into a single platform for everything. By reading the details of each
platform below, and why it is valuable to have each one separate, I hope you'll agree that this is generally a poor
decision, given the trade-offs.

## Platform specifics

### Staff IT Platform

- Purpose:
	- Provide a Staff Identity, usually a User account / profile associated with an email address, that can be used to
	  sign in (e.g. with SSO) and identify a User in ANY other platform, in any environment.
	- Provide staff with basic IT tools, like: Email, Calendar, Staff file storage.
- Business Domain types covered: Generic.
- End Users: All Staff.
- Users access with: Staff Identity (itself).
- Operating staff: An IT department or team of "Workplace" engineers
- Why keep separate from everything else?
	- To maintain the "global" nature of Staff Identity.
		- Identity of staff should be treated as a "Global" thing, where staff don't need special extra accounts to
		  access internal company software for specific environments. This "global" nature of user accounts makes the
		  user experience much better, and designing IAM controls across platforms & environments much more intuitive.
		- A red flag to avoid, in any platform, is the need for a user to "switch" users or accounts when needing to
		  access different platforms or environments. The only thing a user should switch when changing platforms or
		  environments is the URL.
		- All other platforms are more likely to have multiple permanent environments (not just production) used for
		  testing and demos. The purpose behind this platform usually leads to only a production environment existing
		  (or SaaS tools being used) with non-production environments being used for occasional one-off tests, demos or
		  proof of concepts. Allowing Staff Identity to exist on a platform where there are multiple environments will
		  usually lead to multiple Staff identities, one per environment - which results in the loss of the "global"
		  identity benefit.
		- Note: This is about _Identity_ being global and defined centrally. Identity can be federated to different
		  platforms and environments (which is how global identity is implemented). _Access Controls_ for users are
		  managed per environment, per platform.
	- To decouple, via platform segregation, basic IT tooling from all other organisation software, given how critical
	  these tools often are to organisations.
- Non-production environments & identity used for?:
	- Occasional manual testing and demonstrations of Staff IT software (often because this is off-the-shelf software
	  that is otherwise hard to test, without having an environment).
- Usually solved by:
	- SaaS "office" solutions like Google Workspace, Microsoft 365, Odoo, Okta
	- or, IAM solutions like Okta or Keycloak, plus extra software that allows for login using the IAM login.

### Business Management Platform

- Purpose:
	- Provide administrative staff tools to manage the business: Finance (Accounting), Sales, Procurement, Legal, HR,
	  Governance.
- Business Domain types covered: Supporting and Generic.
- End Users:
	- Administrative staff for all.
	- All Staff for some tools (e.g. HR tools).
- Access with: Staff Identity.
- Operating staff: An IT department or team of engineers focused on this administrative software.
- Why keep separate from everything else?
	- Prevent administration work from leaking into operational work (not the same thing).
	- These services tend to scale better than the operational side of the business.
	- Organisations tend to buy or self-host solutions rather than build them themselves for "Business Management"
	  purposes. The combination of functional requirements and non-functional requirements for this platform is
	  therefore often different from a Business Operations Platform.
- Non-production environments & identity used for?:
	- Manual testing and demonstrations of Staff IT software (often because this is off-the-shelf that is otherwise hard
	  to test, without having an environment).
- Usually solved by:
	- Enterprise Resource Planning (ERP) software, like SAP or Oracle software.
	- Dedicated SaaS solutions for different areas of administration.

### Business Operations Platform

- Purpose?
	- Provide software tools to fulfil and/or support the main business purpose of the organisation (i.e. manufacturing
	  goods or providing software services).
	- Provide staff with a means to administrate the software fulfilling and/or supporting the main business purpose of
	  the organisation.
- Business Domain types covered: Core and Supporting, mostly. Generic in some areas.
- End Users:
	- Customers for customer-facing parts.
	- Operational staff for internal-facing parts.
- Access with:
	- Customers: Environment-specific Customer Identity.
	- Operating staff: Staff Identity.
- Operating staff: Operational staff and Software Engineering staff.
- Why keep separate from everything else?
	- This is the software that actually does the work (along with workers) of the organisation. Heavy investment,
	  development, and testing will go into the software running on this platform, and therefore the needs of the
	  platform are more complex. This complexity does not need to be spread into other areas of the organisation, and so
	  a dedicated platform prevents leakage of technical complexity.
		- Complexity examples: Security & Compliance requirements on Customer data; Scalability needs of a business
		  domain that deals with large amounts of data or a global userbase; High criticality & availability needs of
		  core business domains; High desire for maintainability & customisability of the solution; etc.
	- Note: If the organisation is huge in scale, or the complexity of individual business domains is large, it can make
	  sense for particular business domains to have their own Business Operations Platform. This is not usually the
	  case, despite what technologists may believe.
- Non-production environments & identity used for?:
	- Demonstrations.
	- Manual System Integration Testing & User Acceptance Testing (when necessary).
	- Allowing customers or other 3rd parties to test against.
- Usually solved by?
	- Custom-built solutions, hosted on-prem or in the cloud (as appropriate), usually hosted on a Kubernetes cluster or
	  on Virtual Machines (VMs).
	- Bought or open-source self-hosted software, as appropriate for the business domain.
	- SaaS solutions.
	- Service-Oriented and/or Microservice architecture.

### General Data Platform

- Purpose?
	- Provide the organisation the technological means to gather large datasets together for:
		- Analysis for decision-making.
		- Dashboards for monitoring effectiveness and efficiency of the organisation and its operations.
		- Reports & Data Extracts for compliance/regulatory needs.
		- Training AI models for further business automation.
	- Provide Customers with Data Analysis services of their usage of organisations goods/services (if applicable)
- Business Domain types covered: Core and Supporting and Generic.
- End Users:
	- All Staff, depending on their need.
	- Customers (if applicable).
- Access with:
	- Operating staff: Staff Identity.
	- Customers: Environment-specific customer identity (if applicable)
- Operating staff: Operational staff and Software Engineering staff
- Why keep separate from everything else?
	- The non-functional requirements of Data Platform are mostly consistent across different parts of the business
	  (e.g. scalability, responsiveness, availability, security), therefore a single central platform can satisfy all
	  needs (more cost- and resource-effective and results a nicer user experience).
	- Different users may want to combine datasets that source from anywhere in the organisation (whether management or
	  operational). A single platform that holds all data provides for this.
	- Note: If an organisation stores and uses a lot of Customer data in a Data Platform, it can make more sense to
	  split the General Data Platform in two: One for a Customer data and one for Operational & Administrative data.
	  This is because the regulatory requirements on Customer data are usually far greater, resulting in different
	  non-functional requirements (security, privacy, data-retention, etc.) between the two platforms.
- Non-production environments & identity used for?:
	- Testing upgrades and significant changes to the Data Platform that cannot be tested in production.
- Usually solved by?
	- A Data Warehouse and/or Data Lake (or "Data lakehouse").
	- Made from self-hosted open source software and/or a SaaS offering.
	- Data Mesh architecture.

### Internal Developer Platform

aka an IDP.

- Purpose?
	- Provide Software Engineers working within the organisation with the tooling needed to quickly, confidently and
	  effectively develop and ship the software needed by the organisation.
- Business Domain types covered: Generic and Supporting.
- End Users: Software Engineering staff
- Access with: Staff Identity.
- Operating staff: Platform engineers (subset of Software Engineering staff) with contributions from the rest of the
  Software Engineering staff.
- Why keep separate from everything else?
	- The platform has a distinctly different functional purpose from the other platforms.
	- The other platforms require high stability in software solutions for their users, else it is costly (change
	  systems is inherently costly, plus training, plus increase in mistakes/errors). Software Engineering tooling is
	  more susceptible to change and Software Engineers tend to be more adaptable to changing tools due to their greater
	  technical capability than the average user. A platform like this has different non-functionality requirements
	  (adaptability, maintainability, transparency, etc.) than the rest of the platforms.
- Non-production environments & identity used for?:
	- Testing upgrades and significant changes to the Data Platform that cannot be tested in production.
- Usually solved by?
	- Bought or open-source self-hosted software, as appropriate for the business domain.
	- SaaS solutions.
	- Some custom scripts & small tools.

## Platform consolidation vs splitting

All platforms should be/offer the following capabilities for Operating staff:

- Hosting
- Discoverability
- Security
- Observability
- Service-to-Service IAM

Unifying platforms or the above features for "simplicity" is putting "desires for uniformity" above "business purpose"
and actual gained "simplicity" by limiting platform capability only to where it is needed. It is better to split by
purpose and deal with duplication of non-functional capabilities, than it is to unify systems and deal with increased
complexity of having to serve all cross-cutting needs.

E.g. Many organisations / platform engineers may want to unify all observability tooling from all platforms into a
"single pane of glass" view for observability into all platforms because many observability capabilities are shared
across all many or all platforms. It is also an example of executing the "DRY" principle (Don't Repeat Yourself) of
Software Engineering.

Doing this _sounds_ sensible and _feels_ great because it _seems_ more efficient, but entails a great deal of work for
little effort, and means a single observability platform now has to cater for ALL observability needs of all software on
all platforms. However, it is more scalable and easier to focus on purpose by providing the observability capabilities
needed for any specific platform by that specific platform itself. Just because something _sounds_ sensible or more
efficient doesn't mean it actually is.

If managing multiple platforms observability needs becomes overly-cumbersome and the observability capabalities are very
similar between platforms, then it better to invest in _fleet management_ of observability tooling, than it is to invest
in consolidating observability tooling - this results in the same scalability benefits whilst reducing maintenance
efforts.
