# Different Platforms

In Platform Engineering, a platform has the following aspects:

- Some infrastructure for hosting software (applications, databases, blob storage, data warehouses, etc.)
- Some software to handle Identity and Access Management (IAM)
- Environments used for development, testing, operations and demos of software (there should be a minimum of 1 per
  platform: The Production environment)
	- Note: Environments are platform-specific.
- Software Engineers / IT staff using the platform to host software.
- End users of the hosted software.

I have found that it is best to have multiple platforms for specific purposes, where the purpose is usually defined by
whom the End Users are and what service we expect to provide them. This means, in order to know what kind of platform to
build, we need to know how organisations operate.

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

Platforms:

- Staff management platform:
	- End Users: All staff.
	- Users access with: Staff management platform identity (itself).
	- Purpose:
		- Provide a means of identity to all staff, usually a User account / profile associated with an email address,
		  that can be used to sign in (e.g. with SSO) and identify a User in ANY other platform, in any environment.
		- Provide staff with core staff tools, like: Email, Calendar, Expenses, Staff file storage.
	- Business Domain types covered: Generic.
	- Operating staff: An IT department or team of "Workplace" engineers
	- Why keep separate from everything else?
		- Identity of staff should be treated as a "Global" User account, where staff don't need special extra accounts
		  to access internal company software for specific environments. It makes managing IAM for employees much, MUCH
		  simpler.
	- Non-production environments & identity used for?:
		- Manual testing and demonstrations of Staff management software (often because this is off-the-shelf that is
		  otherwise hard to test, without having an environment).
	- Usually solved by:
		- SaaS "office" solutions like Google Workspace, Microsoft 365, Odoo, Okta
		- or, IAM solutions like Okta or Keycloak, plus extra software that allows for login using the IAM login.
- Business management platform:
	- End Users: Administrative staff, and maybe some other staff.
	- Access with: Staff management platform identity.
	- Purpose:
		- Provide administrative staff tools to manage the business: Finance (Accounting), Sales, Procurement, Legal,
		  HR, Governance.
        - TODO: Continue
	- Business Domain types covered: Generic and Supporting.
	- Operating staff: An IT department or team of engineers focused on this administrative software.
	- Why keep separate from everything else?
		- Prevent administration work from leaking into operational work (not the same thing).
		- These services tend to scale better than the operational side of the business.
	- Non-production environments & identity used for?:
		- Manual testing and demonstrations of Staff management software (often because this is off-the-shelf that is
		  otherwise hard to test, without having an environment).
	- Usually solved by:
		- ERP software, like SAP or Oracle software.
		- Dedicated SaaS solutions for different areas of administration.
	- The business management can, and often is, semantically "merged" with the Staff management platform. Pros of doing
	  this is that there is one less platform to deal with. Cons of doing this is that if the business wants to invest
	  more in the "Supporting" business domains by having more custom or dedicated tooling for staff, it is easier to
	  develop this in its own platform than mixing it with the Staff management platform.
- Business operational operations platform:
	- End Users: Customers (and/or Operational staff, if any of the business operations is only internal-facing)
	- Access with:
		- Customers: Environment-specific customer identity.
		- Operating staff: Staff management platform identity.
	- Purpose? TODO: Continue
	- Business Domain types covered: Generic and Supporting and Core.
	- Operating staff: Operational staff and Software Engineering staff.
	- Why keep separate from everything else? TODO: Continue
	- Non-production environments & identity used for?: TODO: Continue
	- Usually solved by? TODO: Continue
- Business operational data platform:
	- End Users: Operational staff (and/or Customers, if part of the Customer-offering is "Data")
	- Access with:
		- Operating staff: Staff management platform identity.
		- Customers: Environment-specific customer identity (if access is offered)
	- Purpose? TODO: Continue
	- Business Domain types covered: Generic and Supporting and Core.
	- Operating staff: Operational staff and Software Engineering staff
	- Why keep separate from everything else? TODO: Continue
	- Non-production environments & identity used for?: TODO: Continue
	- Usually solved by? TODO: Continue
- Internal Developer Platform (IDP):
	- End Users: Software Engineering staff
	- Access with: Staff management platform identity.
	- Purpose? TODO: Continue
	- Business Domain types covered: Generic and Supporting.
	- Operating staff: Platform engineers (subset of Software Engineering staff) with contributions from the rest of the
	  Software Engineering staff.
	- Why keep separate from everything else? TODO: Continue
	- Non-production environments & identity used for?: TODO: Continue
	- Usually solved by? TODO: Continue

## Platform consolidation vs splitting

All platforms should be/offer the following capabilities for Operating staff:

- Security
- Observability
- Service-to-Service IAM

Unifying platforms or the above features for "simplicity" is putting "desires for uniformity" above "business purpose".
It is better to split by purpose and deal with duplication of non-functional capabilities, than it is to unify systems
and deal with increased complexity.

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
