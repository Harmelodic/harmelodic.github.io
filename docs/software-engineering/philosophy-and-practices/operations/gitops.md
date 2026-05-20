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
	- Declarative config & infra is nice to maintain and work with.
	- Ideally transparent, open to all eyes (internally) and so collaboration on infra & config is easier.
- Disadvantages
	- More code to deal with
- General opinion: You should be doing it...
- ...and more than you think. Treating some things as GitOps vs Data, and why you can (and should) be doing it as
  GitOps (e.g. IAM / Auth configuration)
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
