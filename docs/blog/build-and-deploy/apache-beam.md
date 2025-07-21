# Build & Deploy: Apache Beam projects

[Apache Beam](https://beam.apache.org) is an open-source programming model for batch and stream processing (
e.g. [ETL](https://en.wikipedia.org/wiki/Extract%2C_transform%2C_load) processes)

These sorts of processes tend to be [embarrasingly parrallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel)
workloads, and Apache Beam's programming model (and compatible runners) can take advantage of this to achieve
exceptional performance of data processing.

Unlike other server software where build and deployment is done via containerization of applications (
see: [Build & Deploy: Containers](https://harmelodic.com/blog/build-deploy-containers)), Apache Beam projects are
handled a little differently... although not as different as you might think, when deploying to Dataflow.

This blog post covers a high-level view of how Apache Beam *__Java__* projects are built, and then deployed to run on an
Apache Beam Pipeline Runner (specifically, GCP's [Dataflow](https://cloud.google.com/dataflow/docs/overview)).

Apache Beam has alternative SDKs (Python, Go and Scio (Scala)) and Runners (Direct, Flink, Spark, etc.) - however my
experience lies primarily with Java projects and Google Cloud, so I'll be putting my focus on them. The overarching
process should be about the same for SDKs & Runners, but swap out the tooling.

## Building Apache Beam Java projects

Apache Beam projects are essentially just normal Java programs, starting from the `main` method, that construct a
`Pipeline`, before calling `.run()` on that pipeline.

Building Apache Beam projects is essentially the process of taking source code and outputting
a [Pipeline](https://beam.apache.org/documentation/basics/#pipeline).

If we're using Java then that means we're most likely using Maven or Gradle to handle our build (personally, I prefer
Maven and have found Maven to be much more widely used, so I'll be putting more focus on that).

To learn about generically building Maven projects,
see [Build & Deploy: Maven projects](https://harmelodic.com/blog/build-deploy-maven-projects).

Tests are run in the normal way for Java / Maven projects (JUnit and Surefire/Failsafe). There are some useful documents
from both [Apache](https://beam.apache.org/documentation/pipelines/test-your-pipeline/)
and [Google Cloud](https://cloud.google.com/dataflow/docs/guides/develop-and-test-pipelines#test-your-pipeline) for how
to test your pipelines.

Since Apache Beam projects just normal Java programs that run in the `main` method, that we can simply package them as a
JAR. If we were going to deploy to servers with Java & the needed dependencies available, then this can be done with the
`maven-jar-plugin`, but if you want to keep everything together in one bundle, then you can do this with the
`maven-shade-plugin`.

Once the JAR has been built, simply push it to a registry where you can store JARs.

In a professional setup, I would recommend having 1 central registry where you host these JARs, where developers & PRs
can build & publish `SNAPSHOT` versions and only the `main` branch can publish JARs used for production (and production
Runners should only be able to run production JARs).

Before moving on to deploying this Pipeline: When deploying pipelines to a managed solution, like GCP's Dataflow, it is
important to consider how deployment to that managed solution works, and if there are extra or different build steps
that we should add to our build process.

### Building Dataflow Templates

GCP recommends handling the packaging of pipelines
as [Dataflow templates](https://cloud.google.com/dataflow/docs/concepts/dataflow-templates), specifically __Flex
templates__ ("classic" templates are available, but the way to go is Flex templates).

Flex templates are packaged into 2 Artifacts:

- The pipeline, as a container image (surprise, surprise)
- A template specification, as a file in Cloud Storage.

Therefore, we need to be able to build our pipeline into a container image, publish it to a container image registry,
and publish a template specification to Cloud Storage.

In order to build a container image (rather than a JAR as we did before), we can use of the `maven-jib-plugin` (
coincidentally, also built & maintained by Google Cloud).

Once an image has been produced, we need to produce a template specification. Unfortunately, there is no build plugin
available for this for Maven (or Gradle), and instead we must use a `gcloud` command,
as [Google instructs](https://cloud.google.com/dataflow/docs/guides/templates/using-flex-templates#build-template):

```bash
# Reference: https://cloud.google.com/sdk/gcloud/reference/dataflow/flex-template/build
gcloud dataflow flex-template build <args-and-flags>
```

By using this command, and supplying the `--image` flag with the container image that we have built (if you don't supply
this then Google Cloud will attempt to build a fresh image using GCP Cloud Build).

As part of this command we can also supply nearly all the necessary information for the Pipeline will need to run in
Dataflow, as part of the Dataflow Template.  
This means that information only related to the running of the Dataflow Template can be part of the deployment/execution
step (environment information and job-specific arguments).

> ❗️ Important considerations: Don't call `.waitUntilFinish()` after calling `.run()` for Flex Templates - as
> per [Google's comments in their samples](https://github.com/GoogleCloudPlatform/java-docs-samples/blob/main/dataflow/flex-templates/getting_started/src/main/java/com/example/dataflow/FlexTemplateGettingStarted.java#L54).

To further understand the Dataflow Pipeline lifecycle, and be prompted with sensible optimisations to consider, I highly
recommend reading Google
Cloud's [Dataflow Pipeline lifecycle document](https://cloud.google.com/dataflow/docs/pipeline-lifecycle) and
their [Dataflow pipeline best practices document](https://cloud.google.com/dataflow/docs/guides/pipeline-best-practices).

In a professional setup, I recommend having 2 central container image registries (as I explain in
my [Build & Deploy: Containers](https://harmelodic.com/blog/build-deploy-containers) post).  
Similarly, I recommend having 2 template specification buckets for the same reason: One for production template
specifications, one for non-production template specifications. Only production template specifications should be
allowed to be used in production. This improves the security of not allowing anything not built for production to be put
into production.

## Deploying Apache Beam pipelines to Dataflow

When we want to deploy Apache Beam pipelines to runners, we need to ensure we have servers provisioned with "Runner"
software that can execute Apache Beam pipelines. Rather than handle all that, we can take advantage of Cloud Providers
offering managed solutions for deploying & executing our pipelines - Google
Cloud's [Dataflow](https://cloud.google.com/dataflow/docs/overview) is such a product, and what I will be focusing on
here.

As mentioned above, Dataflow templates are the way to go for Google Cloud, so here we're talking about deploying
Dataflow templates, specifically Flex templates.

Assuming we already have a Flex template built (both pipeline container image and template specification) then the only
thing left to do is inform Dataflow that we wish to run that template, which will create a Dataflow "job" that executes
the pipeline on GCP's Dataflow runners.

You can do this by simply calling the Dataflow API. This can be done from code, from the `gcloud` CLI, or via a Cloud
Scheduler operation (built-into Dataflow Pipelines). For example, to call using the `gcloud` CLI, perform:

```bash
# Reference: https://cloud.google.com/sdk/gcloud/reference/dataflow/flex-template/run
gcloud dataflow flex-template run "<job_name>" \
 --template-file-gcs-location "<template-specification-file-in-gcs>" \
 --region "<region>" \
 <further-args-and-flags>
```

> ❗️ Note: We don't specify the container image in this call, because the image is referenced inside the template
> specification file.

---

~ Harmelodic
