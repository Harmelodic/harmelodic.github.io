# Authorising a K8s deployment access to a private container registry

> Originally published: 02 November 2018

When a K8s deployment is created, when creating pods an image will need to be pulled down and used to create containers.

If the Container Registry is private, then the Kubernetes deployment needs credentials, via a K8s secret, that can
access the Container Registry.

First create the Deploy Token (or just get some Registry credentials).

Then create a `docker-registry` K8s secret, e.g.:

```bash
kubectl create secret docker-registry <REPO>-registry-deploy-token \
    --docker-server="registry.gitlab.com" \
    --docker-username="gitlab+deploy-token-xxxxx" \
    --docker-password="xxxxxxxxxxxxxxxxxxxx" \
    --docker-email="xyz@example.com"
```

Ensure your K8s deployment includes your image pull secret, e.g.:

```yaml
apiVersion: apps/v1beta1
kind: Deployment
spec:
  template:
    spec:
      containers:
        - name: example-app
          # ...
      imagePullSecrets:
        - name: <REPO>-registry-deploy-token
```

---

Using a private repository: https://kubernetes.io/docs/concepts/containers/images/#using-a-private-registry

ImagePullSecrets: https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod

Pulling an image from private
repository: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
