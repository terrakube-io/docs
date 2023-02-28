# 🚀 Minikube

Terrakube can be installed in minikube as a sandbox environment to test, to use it please follow this:

### Setup Terrakube namespace

```
kubectl create namespace terrakube
```

### Setup Helm Repository

```
helm repo add terrakube-repo https://AzBuilder.github.io/terrakube
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

### Using Terrakube

The environment has some users, groups and sample data so you can test it quickly.

Visit http://terrakube-ui.minikube.net



You can login using the following user and passwords

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

Select the "simple" organization and the "sample\_simple" workspace and run a job.





{% hint style="info" %}
For more advance configuration options to install Terrkaube visit:

[https://github.com/AzBuilder/terrakube-helm-chart](https://github.com/AzBuilder/terrakube-helm-chart)
{% endhint %}
