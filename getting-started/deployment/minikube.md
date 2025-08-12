# Minikube

{% hint style="warning" %}
This following will install Terrakube using "HTTP" a few features like the Terraform registry and the Terraform remote state won't be available because they require "HTTPS", to install with HTTPS support locally with minikube check [this](minikube-+-https.md)
{% endhint %}

Terrakube can be installed in minikube as a sandbox environment to test, to use it please follow this:

### Setup Helm Repository

```
helm repo add terrakube-repo https://charts.terrakube.io
helm repo update
```

### Setup Minikube

```
minikube start
minikube addons enable ingress
minikube addons enable storage-provisioner
minikube ip 
```

{% hint style="info" %}
Copy the minikube ip address ( Example: 192.168.59.100)
{% endhint %}

### Setup Terrakube namespace

```
kubectl create namespace terrakube
```

### Setup DNS records

```
sudo nano /etc/hosts

192.168.59.100 terrakube-ui.minikube.net
192.168.59.100 terrakube-api.minikube.net
192.168.59.100 terrakube-reg.minikube.net
```

### Install Terrakube

```
helm install terrakube terrakube-repo/terrakube -n terrakube
```

{% hint style="info" %}
It will take some minutes for all the pods to be available, the status can be checked using

kubectl get pods -n terrakube
{% endhint %}

{% hint style="warning" %}
If you found the following message "_**Snippet directives are disabled by the Ingress administrator**_", please update the _**ingres-nginx-controller configMap in namespace ingress-nginx adding the following:**_

```
allow-snippet-annotations: "true"

Reference: https://github.com/terrakube-io/terrakube/issues/618#issuecomment-1838980451
```
{% endhint %}

The environment has some users, groups and sample data so you can test it quickly.

Visit http://terrakube-ui.minikube.net and login using admin@example.com with password admin

<figure><img src="../../.gitbook/assets/image (312).png" alt=""><figcaption></figcaption></figure>

You can login using the following user and passwords

<figure><img src="../../.gitbook/assets/image (296).png" alt=""><figcaption></figcaption></figure>

| User              | Password | Member Of                               |
| ----------------- | -------- | --------------------------------------- |
| admin@example.com | admin    | TERRAKUBE\_ADMIN, TERRAKUBE\_DEVELOPERS |
| aws@example.com   | aws      | AWS\_DEVELOPERS                         |
| gcp@example.com   | gcp      | GCP\_DEVELOPERS                         |
| azure@example.com | azure    | AZURE\_DEVELOPERS                       |

| Groups                |
| --------------------- |
| TERRAKUBE\_ADMIN      |
| TERRAKUBE\_DEVELOPERS |
| AZURE\_DEVELOPERS     |
| AWS\_DEVELOPERS       |
| GCP\_DEVELOPERS       |

{% hint style="warning" %}
The sample user and groups information can be updated in the kubernetes secret:

terrakube-openldap-secrets
{% endhint %}

{% hint style="danger" %}
Minikube will use a very simple OpenLDAP, make sure to change this when using in a real kubernetes environment. Using the option security.useOpenLDAP=false in your helm deployment.
{% endhint %}

Select the "simple" organization and the "sample\_simple" workspace and run a job.

<figure><img src="../../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (245).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (288).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
For more advance configuration options to install Terrakube visit:

[https://github.com/terrakube-io/terrakube-helm-chart](https://github.com/terrakube-io/terrakube-helm-chart/)
{% endhint %}
