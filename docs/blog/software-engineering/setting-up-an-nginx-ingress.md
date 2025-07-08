# Setting up an NGINX ingress

> Originally published: 03 November 2018

## Requirements

- `kubectl` - connected to your cluster.
- `helm`

## Installation

Install `tiller` on the cluster:

```bash
helm init
```

Setup the default kube-system service account as a cluster admin, this allows tiller to deploy things:

```bash
kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default
```

Install the NGINX Ingress Controller

```bash
helm install --name nginx --set rbac.create=true stable/nginx-ingress
```

Check the nginx-ingress is up by doing:

```bash
kubectl get services
```

In any `ingress.yaml` files for any applications you make, ensure that under `metadata > annotations`, you define the
`ingress.class` as `"nginx"`.  
For example:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-app
  namespace: example-namespace
  labels:
    app: example-app
  annotations:
    kubernetes.io/ingress.class: "nginx"
```

## TLS

_Taken from the output of installing the nginx-ingress._

An example Ingress that makes use of the controller:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  name: example
  namespace: foo
spec:
  rules:
    - host: www.example.com
      http:
        paths:
          - backend:
              serviceName: exampleService
              servicePort: 80
            path: /
  # This section is only required if TLS is to be enabled for the Ingress
  tls:
    - hosts:
        - www.example.com
      secretName: example-tls
```

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: example-tls
  namespace: foo
data:
  tls.crt: <base64 encoded cert>
  tls.key: <base64 encoded key>
type: kubernetes.io/tls
```
