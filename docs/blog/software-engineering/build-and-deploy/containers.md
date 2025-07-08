# Build & Deploy: Containers

Containers are the output of software going
through [Containerization](https://en.wikipedia.org/wiki/Containerization_(computing)).

Important disambiguation:

- A *container image* is the deployable artifact that contains the software that you wish to execute.
- A *container* is an actively-executing instance of that container image.

The term "containers" is sometimes used as an umbrella term for both container images and containers (as I have done in
the title of this blog post). I will endeavour to not do that in the rest of the blog post.

This post mainly focuses on the deployment of container images, with a brief section on how to generically build a
container image.

> ❗️ Note: When containers became a huge deal in the 2010s, most people started packaging their software
> into [Docker](https://www.docker.com) container images, as the Docker company were industry-leaders at the time and thus
> defined the standards for building container images and running containers.  
> Due to a variety of reasons, the industry has begun shifting to using
> the [Open Container Initiative (OCI)](https://en.wikipedia.org/wiki/Open_Container_Initiative) standard for building and
> interacting with container images & containers. Even Docker (the company) now provides the tooling/options to build OCI
> container images.

## Building Container Images

To package an application, we should write some code to define how the packaging should be done, and then be able to run
a command that executes that code.

There are a variety of ways to build a container image, for different project types. For those more "specialised"
methods, I will cover those in dedicated blog posts for those project types. I recommend using those over the more "
generic" build method described below.

For generically packing applications into container images, we write our instructions in
a [Dockerfile](https://docs.docker.com/reference/dockerfile/) and then run a CLI tool such as `docker` or `podman` to
take that `Dockerfile` and build the image.

Container images are packaged into layers. When we want to build a container image, 99% of the time we will take an
existing layer and then write our own layer on top of that, which does the containerization of our application. In a
`Dockerfile` that is done by defining a `FROM` command.

After the `FROM` command, we can define lots of different commands that perform the tasks to package your application
into a container image. For example, if you had a folder in your repo called `public` that contained files for hosting
on a NGINX webserver, and had a custom NGINX config file defined as `nginx.conf`, you might write a Dockerfile that
looks something like:

```Dockerfile
FROM nginx:1.27.0

COPY public/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf
```

For security (and efficiency) reasons, I recommend using
a [distroless](https://github.com/GoogleContainerTools/distroless) container image as the image you define in your
`FROM` command.

Once you have your Dockerfile defined, you can build your container image by using a CLI tool, for instance
with [podman](https://docs.podman.io):

```bash
podman build -t my-image:1.0.0 .
```

This will look in `.` (current directory) for a `Dockerfile`, and then build your image on the local machine, naming it
`my-image` and tag with version `1.0.0` - Versioning can depend on your use case, but generally I recommend sticking
to [Semantic Versioning](https://semver.org).

In order to publish your image, you need to push it to a container image registry (such
as [Dockerhub](https://hub.docker.com) or GCP's [Artifact Registry](https://cloud.google.com/artifact-registry)). You'll
need to do some preconfiguration & special tagging in order to prepare your image for pushing to the registry of your
choice, but once done, it should be as simple as then running:

```bash
podman push <image>:<tag>
```

When setting up a container image registry for a organisation's container images, I recommend setting up at least 2
container image registries. One for container images that are to be deployed to production environments, and one for all
other container images (dev-built images). Doing this allows you to do some sensible security-hardening:

1. Ensure only specific parts of your build systems are allowed to push to the container image registry for production
   images.
2. Ensure your container runtime system (see below) is allowed to pull from the container image registry for production
   images.

This ensures that no unauthorized images can be run in production environments.

Alternatively, if you are only running 1 environment (production) then you can have 1 container image registry, and
developers/PRs/etc. don't & cannot push to it.

## Deploying Container Images

There are multiple ways to deploy container images and run containers.

Most use-cases for running containers involves running them on servers. That is what I'll be focusing on in this post,
in which case there are basically two options available to you:

1. Use [Kubernetes](https://kubernetes.io) to "orchestrate" your containers on a cluster of servers, with continous
   deployment being handled by a GitOps continuous delivery (CD) system such
   as [Argo CD](https://argoproj.github.io/cd/) or [Flux CD](https://fluxcd.io/).
2. Run your container in a "serverless" solution such as GCP's [Cloud Run](https://cloud.google.com/run),
   AWS's [App Runner](https://aws.amazon.com/apprunner/), or
   Azure's [Container Apps](https://learn.microsoft.com/en-us/azure/container-apps/overview), where deployment is
   handled via some sort of infrastructure as code (e.g. Terraform).

There are absolutely other alternatives to running containers (docker compose on a VM, just running containers manually
in VMs, cloud-provider-specific container-orchestration systems) but ultimately the industry standard solution for
managing containers *is* Kubernetes, and the serverless solutions exist for when you don't want to or need to actively
manage a cluster of servers yourself.

From here I will describe two solutions, and the solutions I actually recommend:

__Kubernetes with Argo CD__ for deploying a lot of containers to a cluster of servers, and fine-control over those
deployed containers is readily availble.

__GCP's Cloud Run, with Terraform__ for quickly deploying a container with basic needs.

### Kubernetes with Argo CD

To run containers on servers, you need to have servers. These servers should probably be clustered together, so that you
can interact with them as one entity rather than as separate entities. You should be able to interact with those servers
in a nice way in order to run containers on them and scale those containers, if needed. It'd be nice if those servers
recognised if one of the containers was failing and thus automatically restarted it or took the container out of being
active in the network. Speaking of networks, it'd be nice if the containers were networked together in a nice way, and
could interact and communicate with one another easily.

All of that work can be described as "container orchestration".

[Kubernetes](https://kubernetes.io) is a "container orchestration" system, for automating the deployment, scaling and
management of containers.

You can run containers on all sorts of machines and clusters of machines, and using Cloud Providers. Different Cloud
Providers provide "managed" versions of Kubernetes, where you don't even need to worry about how to upgrade to new
versions of Kubernetes or manage the Kubernetes Control Plane servers - e.g.
GCP's [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine).

Once you have a GKE cluster provisioned, you need to be able to deploy containers to that cluster.

Initially, this is possible by simplying applying "Kubernetes resources" to your Kubernetes server. These Kubernetes
resources are simply configuration that the Kubernetes system will use in order to run and manage your containers.  
The main resource you will likely need to do use is
a [Deployment](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/deployment-v1/), though other
sorts of "workload" resources are available.  
If your container needs to be communicated with then other Kubernetes resources that handle cluster networking &
communication can be used, such as
the [Service](https://kubernetes.io/docs/reference/kubernetes-api/service-resources/service-v1/)
and [Ingress](https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-v1/) resources.  
Further configuration and advanced configuration can be found in
the [Kubernetes reference documentatation](https://kubernetes.io/docs/reference/).

Manually deploying these resources can be a pain. Automation to the rescue! You could store your Kubernetes resources in
a Git repo and then setup automation stuff (e.g. in GitHub Actions) to apply those resources to your Kubernetes cluster,
but... what if you delete a Kubernetes resource from your Git repo? How do you ensure that it is removed from your
Kubernetes clsuter? Or what if you haven't made a change in a while, and one of the resources gets deleted for some
reason (hacking/accident)? How can we ensure that the resource is recreated?  
It'd be better to have a system in place that continuously checks that all the Kubernetes resources in your Git repo are
applied to your Kubernetes cluster. This sort of thinking is called "GitOps", because we're handling our Operations
through Git.

[Argo CD](https://argoproj.github.io/cd/) is a GitOps continuous delivery tool for Kubernetes. After being setup and
configured to look at your Git repositories and Kubernetes cluster(s), it will ensure that the two are in sync, always
taking the Kubernetes resources in your Git repository as the source of truth.

Argo CD uses its [Application](https://argo-cd.readthedocs.io/en/stable/user-guide/application-specification/) resource
in order to evaluate where collections of Kubernetes resources are found in order to deploy systems. Since the
Application resource is a type of Kubernetes resource, you can build a tree of Applications, being sourced from other
Applications. This is referred to as
the [app of apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/).

Argo CD itself runs on Kubernetes, and can be installed via a Helm Chart. When building new Kubernetes systems, I tend
to provision Kubernetes with Terraform code, and then install the Argo CD Helm Chart via Terraform, the deploy a root
Argo CD application via Terraform, and let Argo CD then handle the deployment from there.

Wait... Helm Chart? What's that???

#### Helm Charts & Kustomize

Often a system your building will be made up of multiple Kubernetes resources, possibly even multiple containers and
deployments. You might even need to deploy all these resources to different environments, and so don't want to
copy-paste the resources all over the place.

Therefore bundling these resources together into a single deployable entity becomes a nice way of maintaining your
Kubernetes resources. There are two ways to bundle your Kubernetes resources together:

A [Helm Chart](https://helm.sh/docs/topics/charts/), which packages together "templates" of a collection of Kubernetes
resources, which you can then inject values into those templates as needed for an environment.

A [Kustomize](https://kubectl.docs.kubernetes.io/references/kustomize/) Kustomization, which packages together the
collection of Kubernetes resources as a "base" set of resources, which then allows for "overlays" to customise & patch
the base set of resources as needed for an environment.

If you're building and running your own applications and not providing them to others,
then [Kustomize](https://kubectl.docs.kubernetes.io/references/kustomize/) is a better solution, where each overlay
corresponds to an environment you need to run in. This keeps the Kubernetes resources simple and easy to read & maintain
as the need for heavy customisability will be low, because you control the context of your applications.

If you're building a system for other people to use on their Kubernetes setups (e.g. an open-source project), then
a [Helm Chart](https://helm.sh/docs/topics/charts/) is a better option, as it can be customized, versioned and shipped
as its own entity. Publicly available Helm Charts are available on [Artifact Hub](https://artifacthub.io/).

> ❗️ Note: Helm Charts have recently started supporting being packaged according to the OCI standard - meaning you can
> start treating Helm Charts like any other OCI image and storing them in an OCI image registry (like container images).

> ❗️ Note: Helm (the CLI tool that can install & manage Helm Charts) has its own way to managing the lifecycle &
> upgrading of a Helm Chart. Some CD systems like Flux CD support and work with this lifecycle process, others like Argo
> CD do not and simply evaluate what a Helm Chart would create and then creates those resources. I prefer the Argo CD way,
> but if you want to use Helm's lifecycle, then Flux CD would be a better option for you.

### Cloud Run with Terraform

Cloud Run is a "serverless" solution for running containers - which basically means that rather than setting up
servers/VMs and installing software on them to be able to run your container, you instead just provide a Cloud
provider (in this case GCP) some basic deployment configuration that defines what containers you'd like to run and how,
and the Cloud provider will do the rest.

Incidentally, in the case of Cloud Run, Google are basically just running Kubernetes behind the scenes.

I won't go into depth on how to best handle deploying/executing Terraform code, as I'll leave that for another blog
post.

To deploy something to Cloud Run via Terraform, you should define your Cloud Run deployment configuration in Terraform
code using
the [google_cloud_run_v2_service](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/cloud_run_v2_service)
resource. Then apply your Terraform code!

You can read more about doing further configuration of your Cloud Run deployment, in
the [Cloud Run docs](https://cloud.google.com/run/docs/overview/what-is-cloud-run).

~ Harmelodic
