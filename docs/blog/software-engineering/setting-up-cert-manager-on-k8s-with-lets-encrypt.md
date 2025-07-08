# Setting up cert-manager on K8s with Let's Encrypt

> Originally published: 18 November 2018

Cert-manager is a certificate management controller for Kubernetes (K8s).

You can connect it up to Let's Encrypt and other services to enable HTTP over TLS (HTTPS). This doc details how to set
it up on Kubernetes for the domain `example.com`.

## Notes

This uses the [DNS-01 validation](http://docs.cert-manager.io/en/latest/reference/issuers/acme/dns01.html) challenge,
which allows us to obtain wildcard certificates.  
You can use [HTTP-01 validation](http://docs.cert-manager.io/en/latest/reference/issuers/acme/http01.html), however,
cert-manager will not support obtaining wildcard certificates through HTTP-01.  
This guide also only covers
Route53; [Other providers are available](http://docs.cert-manager.io/en/latest/reference/issuers/acme/dns01.html#supported-dns01-providers).

This requires you to be using K8s 1.9+ as RBAC is required.

This guide details how to setup cert-manager to work with NGINX Ingress. Other Ingress solutions/architectures may work
differently.

## Guide

1. Ensure you have Helm installed on your machine.

2. Install Tiller on your cluster:

   ```bash
   helm init
   ```

3. Give Tiller the RBAC cluster-admin permissions it needs:

   ```bash
   kubectl create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default
   ```

4. Install cert-manager using Helm:

   ```bash
   helm install \
       --name cert-manager \
       --namespace kube-system \
       stable/cert-manager
   ```

5. Prove it's started with:

   ```bash
   kubectl -n kube-system get pods
   ```

   you should see:

   ```
   NAME                                               READY     STATUS    RESTARTS   AGE
   cert-manager-xxxxxxxxxx-xxxxx                      1/1       Running   0          46s
   ...
   ```

6. Fetch your Hosted Zone ID by going to: `AWS > Route53 > Hosted zones` and copying the Hosted Zone ID from your
   appropriate domain name. (Here on out, the example Hosted Zone ID will be `EGZONEIDFROM53`)

7. Setup a Route53 IAM Policy by going to: `AWS > IAM > Policies > Create policy > JSON` and pasting:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "route53:GetChange",
               "Resource": "arn:aws:route53:::change/*"
           },
           {
               "Effect": "Allow",
               "Action": "route53:ChangeResourceRecordSets",
               "Resource": "arn:aws:route53:::hostedzone/EGZONEIDFROM53"
           }
       ]
   }
   ```

   Name this policy: `CertManager-DNS01-Validation-example.com`

8. Create a new AWS user for cert-manager to use to perform DNS-01 validation by going to `AWS > IAM > Users > Add User`
   and creating a new user with the following properties:
	- Programmatic access
	- Name this user `cert-manager-example.com`.
	- In Permissions, choose `Attach existing policies directly` and select the
	  `CertManager-DNS01-Validation-example.com` policy we just created.

   Copy the Access Key ID and Secret Access Key generated.  
   Here on out, these will be referred to as `ACCESSKEYID` and `SECRETACCESSKEY`, respectively.

9. Create a secret to hold the Secret Access Key:

   ```bash
   kubectl create secret generic cert-manager-route53 --from-literal=secret-access-key=SECRETACCESSKEY
   ```

10. Create an Issuer (this provides the interface to your CA, from which x509 certificates can be obtained):

	```yaml
	apiVersion: certmanager.k8s.io/v1alpha1
	kind: Issuer
	metadata:
	  name: letsencrypt-staging
	spec:
	  acme:
		server: https://acme-staging-v02.api.letsencrypt.org/directory
		email: user@example.com
		privateKeySecretRef:
		  name: letsencrypt-staging
		dns01:
		  providers:
		  - name: route53
			route53:
			  region: eu-west-1
			  hostedZoneID: EGZONEIDFROM53
			  accessKeyID: ACCESSKEYID
			  secretAccessKeySecretRef:
				name: cert-manager-route53
				key: secret-access-key
	```

	Note: You can create a ClusterIssuer, which is the same as an Issuer except that it will issue certificates across
	_all_ K8s namespaces, whereas a normal Issuer will only issue certificates to the namespace it is deployed in.  
	If you choose to use a ClusterIssuer, you will have to rename all references to `Issuer` to `ClusterIssuer` and all
	references to `certmanager.k8s.io/issuer` to `certmanager.k8s.io/cluster-issuer`, from here on out.

	Note: You'll have noticed we are providing AWS credentials here. However, this is optional as cert-manager, when
	using the Route53 provider, can
	use [ambient credentials](http://docs.cert-manager.io/en/latest/reference/issuers.html#ambient-credentials), but
	only under certain circumstances.

	Note: If you're using a K8s Service Account (e.g. within a CI/CD process) to create this issuer, you'll need to bind
	the `cert-manager` ClusterRole to the Service Account.  
	For example, if you're using a GitLab CI/CD with a K8s Service Account called `gitlab` in the `default` namespace
	you'd perform:

	```bash
	kubectl create rolebinding gitlab-cert-manager --clusterrole=cert-manager --serviceaccount=default:gitlab
	```

11. Create an NGINX Ingress with the following extra values:

	```yaml
	# ...
	metadata:
	# ...
	annotations:
	  # ...
	  kubernetes.io/tls-acme: "true"
	  certmanager.k8s.io/acme-dns01-provider: route53
	  certmanager.k8s.io/acme-challenge-type: dns01
	  certmanager.k8s.io/issuer: letsencrypt-staging
	# ...
	spec:
	  # ...
	  tls:
	  - hosts:
		- "*.example.com"
		- example.com
		secretName: tls-staging-cert-wildcard-example
	```

12. Wait for the Issuer to go perform the validation checks and obtain certificates.  
	After about 5 minutes, perform:

	```bash
	kubectl get secrets
	```

	and you should see a new secret called `tls-staging-cert-wildcard-example` of type `kubernetes.io/tls`.

	If you access `https://example.com`, you will fail the browser's trust, but if you check the site's certificate you
	should see that the certificate is a `Fake LE Root X1` certificate.

	This means that your cert-manager & Issuer configuration is working

13. Configure for production by removing `-staging` from all places in your Issuer & Ingress YAML configurations; Then
	redeploy the Issuer and Ingress, wait a few minutes and access your newly secured and trusted HTTPS domain.
