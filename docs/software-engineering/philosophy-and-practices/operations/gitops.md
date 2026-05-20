# GitOps

Declare config & infrastructure as code and store in Git. Use a deployment system (e.g. Argo CD, Flux, Atlantis) to make
what is in the code match what exists in real life.

Usually have some kind of reconciliation involved so that when real life "drifts" from what is defined, then the
reconciliation system "fixes" it back to being correct, ideally automatically on a regular interval. (Some CD systems,
like Argo CD and Flux, are designed to reconcile/sync by default).

Use cases:

- Any Kubernetes or Kubernetes-related resources (e.g. Deployments, Services, CronJobs, Istio/Service Mesh config,
  Prometheus/Metrics-scraper config, Ingress config, etc.)
- Infrastructure as Code (Terraform, Crossplane (Infra as Kubernetes), CloudFormation, etc.)
- Component inventory metadata code (e.g. files that contain metadata about applications, libraries, etc. for what is
  found in different Git repositories). This could be read or deployed by component inventory or visualisation systems.

TODO more on:

- Advantages
	- Easy to recreate everything from scratch (great for disaster recovery, but also development environment creation,
	  and proving environments can be easily and quickly recreated).
	- Declarative config & infra is nice to maintain, refactor, and work with.
	- Ideally transparent, open to all eyes (internally) and so collaboration on infra & config is easier.
	- Encourages an attitude towards config / infrastructure can be ephemeral/recreatable which makes choosing where you
	  store your state as an easier and more deliberate choice.
		- This combined with my semi-critical take on DLQs means I tend to treat Message Bus/Queues as stateful whilst
		  they have messages but otherwise more like stateless systems, meaning I prefer handling state with databases
		  and having backups, rather than having DLQs and letting messages sit in Buses/Queues for a long time.
	- Easier to handle single source of truth as Configuration that then reconciles (and doing ID/string matching based
	  on standard naming when referencing) rather than treating it as data and building complex event-driven
	  architectures (some event-driven stuff may still be needed for GitOps though, depending on what you're doing).
- Disadvantages
	- More code to deal with - though this is made substantially easier by fleet management practices (like MR/PR
	  Campaigns, automatic dependency upgrades (Renovate), and
	  other [automated code refactoring](../development/automated-code-refactoring.md))
	- Reconciling Stateful infrastructure can be... weird (if there's data involved, and you deleted a thing, you should
	  probably restore rather than recreate? But caches you can just recreate? Or you might need to recreate before you
	  restore? It all depends on the provider and the kind of stateful infrastructure).
	- Reconciling or recreating some infrastructure with some providers has... quirks (e.g. deleting a database might
	  not actually delete it, and so recreating it doesn't actually work, so you need to think a lot about naming and
	  reproducibility of systems more than you would if you were not doing GitOps and just did backups)
- General opinion: You should be doing it...
- ...and more than you think. Treating some things as GitOps vs Data, and why you can (and should) be doing it as
  GitOps (e.g. particularly around IAM configuration - Service Accounts, Access Controls, even Employee User Accounts).
- Some things to think about when implementing:
	- Labelling deployed config/infrastructure/stuff what Git repo and what commit is currently deployed, to be able to
	  trace back to the code easily.
	- Reconcile the ephemeral / stateless stuff (Deployments, Services, most Kubernetes config, some infrastructure
	  (e.g. networking)).
	- Probably don't auto-reconcile the infrastructure / stateful stuff, but check for drift regularly and alert if
	  drifting.
	- When deploying infrastructure:
		- Plan in branch (block other branch's possible plans), approve/pass checks, trigger apply in branch, block
		  other applies from happening in repo, start apply.
		- If successful apply, merge to trunk to keep in sync.
		- If unsuccessful apply, don't merge to trunk, and re-apply trunk to reset existing infrastructure to match
		  trunk.
