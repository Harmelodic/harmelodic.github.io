# Platform Overview

a.k.a. Platform Engineering

- General
	- Infra as code
	- zero trust
	- IAM access
	- environments
	- observability
	- DNS & networking
	- improving developer experience (aspects of making: new, improve, maintain, move, operate/observe/optimise, sunset,
	  collab, discuss, decide)
- Compute platform (compute options (serverless, kubernetes, VMs, hardware), service-mesh, ingress/egress, LBs,
  certificates)
- CI/CD / Supply-chain platform (git, build system, contracts, artifact storage (+ mirroring public repos), security
  analysis, deployment systems, SBOM generation & upload (CycloneDX standard))
- Internal Development Tools platform (sourcegraph, developer portal, templating, documentation, SBOM storage &
  exploration)
- Big Data (storage, normalisation, access)

## Cloud Providers

- [AWS - Amazon Web Services](https://aws.amazon.com/)
- [Cloudflare](https://www.cloudflare.com/)
- [DigitalOcean](https://www.digitalocean.com/)
- [GCP - Google Cloud Platform](https://cloud.google.com/)
- [Google API Client Libraries](https://developers.google.com/api-client-library/)
- [Heroku](https://www.heroku.com/)
- [Microsoft Azure](https://azure.microsoft.com/en-gb/)
- [Nextcloud](https://nextcloud.com/)
- [Openstack](https://www.openstack.org/)
- [tsoHost (UK only)](https://www.tsohost.com/)

## Networking

- [Consul - Service Discovery](https://www.consul.io/)
- [Fastly](https://www.fastly.com/)
- [HTTP/1](https://en.wikipedia.org/wiki/HTTP)
- [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2)
- [HTTP/3](https://en.wikipedia.org/wiki/HTTP/3)
- [Istio - Service Mesh](https://istio.io/)
- [Kubernetes Ingress-NGINX Controller](https://kubernetes.github.io/ingress-nginx/)
- [NGINX Docs](https://nginx.org/en/docs/)
- [SMP - Service Mesh Performance](https://smp-spec.io/dashboard)
- [Varnish HTTP Cache](https://varnish-cache.org/)

## CI / CD Systems

- [Argo CD](https://argo-cd.readthedocs.io/en/stable/)
- [Bamboo - Atlassian](https://www.atlassian.com/software/bamboo)
- [CircleCI](https://circleci.com/)
- [GitHub Actions](https://github.com/features/actions)
- [GitLab CI](https://about.gitlab.com/solutions/continuous-integration/)
- [Helm - Kubernetes Deployments](https://helm.sh/)
- [Jenkins](https://www.jenkins.io/)
- [Jenkins X](https://jenkins-x.io/)
- [Kustomize - Kubernetes Deployments](https://kustomize.io/)
- [TeamCity - Jetbrains](https://www.jetbrains.com/teamcity/)
- [Travis CI](https://www.travis-ci.com/)

## Observability

- [Alertmanager - Prometheus](https://prometheus.io/docs/alerting/latest/alertmanager/)
- [Elasticsearch](https://www.elastic.co/products/elasticsearch)
- [Google Analytics](https://analytics.google.com)
- [Grafana](https://grafana.com/)
- [Jaeger Tracing](https://www.jaegertracing.io/)
- [Kibana](https://www.elastic.co/products/kibana)
- [Kubeapps Dashboard](https://kubeapps.dev/)
- [Logstash](https://www.elastic.co/products/logstash)
- [Micrometer](https://micrometer.io/)
- [OpenTelemetry](https://opentelemetry.io/)
- [Opsgenie](https://www.atlassian.com/software/opsgenie)
- [Prometheus](https://prometheus.io/)
- [Splunk](https://www.splunk.com/)
- [UptimeRobot](https://uptimerobot.com/)
- [fluentd](https://www.fluentd.org/)

## General Platform Engineering

- [Ansible](https://www.ansible.com/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)
- [Puppet](https://puppet.com/)
- [Terraform](https://www.terraform.io/docs/)
- [kOps](https://github.com/kubernetes/kops)
- [Vagrant](https://www.vagrantup.com/)
- [Apache Openwhisk - Serverless Cloud Platform](https://openwhisk.apache.org/)
- [Serverless Framework](https://serverless.com/framework/docs/)

## Artifact Repositories

- [Artifact Registry - Google Cloud](https://cloud.google.com/artifact-registry)
- [ArtifactHub - Kubernetes Packages](https://artifacthub.io/)
- [Docker Hub](https://hub.docker.com/)
- [GitHub Releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
- [GitLab Releases](https://docs.gitlab.com/ee/user/project/releases/)
- [JFrog Artifactory](https://jfrog.com/artifactory/)
- [npm - JavaScript packages](https://www.npmjs.com/products)
- [Maven Repository](https://mvnrepository.com/)
- [Maven "Central" Repository](https://central.sonatype.org/)
- [Sonatype Nexus OSS](https://www.sonatype.com/products/repository-oss)

## Authentication / Authorization / IAM

- [Auth0](https://auth0.com/)
- [Crowd - Atlassian](https://www.atlassian.com/software/crowd)
- [GitHub's OpenID Configuration](https://token.actions.githubusercontent.com/.well-known/openid-configuration)
- [JWT - JSON Web Tokens](https://jwt.io/)
- [Keycloak](https://www.keycloak.org/)
- [LDAP - Lightweight Directory Access Protocol](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol)
- [Workload Identity (GKE)](https://cloud.google.com/kubernetes-engine/docs/concepts/workload-identity)

## Certificate Management

- [cert-manager](https://cert-manager.io/)
- [digicert - CA](https://www.digicert.com/)
- [GlobalSign - CA](https://www.globalsign.com/)
- [GoDaddy - SSL Certificates](https://uk.godaddy.com/web-security/ssl-certificate)
- [Let's Encrypt - CA (nonprofit)](https://letsencrypt.org/)
- [The SSL Store](https://www.thesslstore.com/)

## Performance Testing

- [Artillery](https://www.artillery.io/)
- [JMeter](https://jmeter.apache.org/)
- [k6](https://k6.io/)
- [Locust](https://locust.io/)j
- [Testkube](https://testkube.io/)
- [UL Benchmarks](https://benchmarks.ul.com/)

## SEO & Offsite UX

- [Dark Visitors - Robots.txt Agents](https://darkvisitors.com/agents)
- [Meta Description Tag](https://moz.com/learn/seo/meta-description)
- [OGP - Open Graph Protocol](https://ogp.me/)
- [Summary Card - Twitter/X](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/summary)

## Google Cloud / Workspace Tools

- [Google Admin SDK API](https://developers.google.com/admin-sdk/overview)
- [Google Workspace APIs Explorer](https://developers.google.com/workspace/explore?filter=)
- [OAuth 2.0 - Google Playground](https://developers.google.com/oauthplayground/)
- [Understanding Roles - GCP](https://cloud.google.com/iam/docs/understanding-roles)
- [gcloud - CLI Reference](https://cloud.google.com/sdk/gcloud/reference)
- [gsutil - CLI Reference](https://cloud.google.com/storage/docs/gsutil)

## Password Management

- [1Password](https://1password.com/)
- [Bitwarden](https://bitwarden.com/)
- [Dashlane](https://www.dashlane.com/)
- [KeePass](https://keepass.info/)
- [LastPass](https://www.lastpass.com/)
- [Passkeys - Apple Developer](https://developer.apple.com/passkeys/)
- [Passkeys - FIDO Alliance](https://fidoalliance.org/passkeys/)
- [Passkeys - Google Developers](https://developers.google.com/identity/passkeys)
