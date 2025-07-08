# My personal CI/CD setup (2024)

> Originally published: 18 May 2024

Most software I make for myself is web stuff:

- "Frontend" web apps (with JavaScript, npm, React etc.)
- "Backend" services (with Java, Maven, Spring, etc.)
- Cloud infrastructure (with GCP, Terraform, etc.)

But I've also got some little hobby projects here and there that aren't that.

I __*build*__ this stuff (CI) in GitHub Actions using the respective build tool for each ecosystem I interact with:

- For Java, I use Maven with JIB to make container images.
- For JavaScript, I use npm with a Dockerfile to make container images.
- For Terraform... you don't build it, just validate & deploy it using `terraform` commands.

Some times this interacts with my [Pact Broker](https://pact.harmelodic.com), if I've written contract testing.

and I __*deploy*__ this stuff (CD) with:

- GitHub Actions, to Terraform code against GCP.
- Argo CD, for containers deploying to Kubernetes.

I have an [Argo CD](https://argo-cd.harmelodic.com) instance running, which picks up changes in GitHub and deploys them.
I'm lazy right now and just set the container version to `latest`, but I could set it up to just version the
containers (probably with commit SHAs) if I wanted to do things more "properly".

I could run an Atlantis/Scalr/TFC deployment system for my Terraform, but... I really don't need that - running some
terraform commands in a GitHub Action works just fine, and my GitHub Actions are configured to use Workload Identity, so
I'm not passing any Service Account Keys around.

I've got some improvements to makes, but honestly this works pretty well, and reflects a very basic version of what I've
used in professional scenarios, so it keeps me pretty sharp with "how things are done".
