# Azure Active Directory

### &#x20;Requirements

* Azure Active Directory.
* Azure Active Directory group with name "TERRAKUBE\_ADMIN"
* Public DNS.
* Kubernetes with Nginx Ingress installed and working.
* [Helm](https://helm.sh/)
* Kubernetes namespace named "terrakube"

{% hint style="info" %}
If you are using Azure Kubernetes Service the following information can be usefull to setup the ingress controller with TLS. [Link](https://learn.microsoft.com/en-us/azure/aks/ingress-tls?tabs=azure-cli)
{% endhint %}

### DNS Records

For this example we will have to update our DNS records adding the following domains and the public IP address of the nginx ingress controller in our kubernetes cluster:&#x20;

* terrakube-registry.sandbox.terrakube.io
* terrakube-ui.sandbox.terrakube.io
* terrakube-api.sandbox.terrakube.io

### Setup Azure Authentication

You will need to complete the Azure authentication setup for Dex.&#x20;

{% hint style="success" %}
Dex configuration information can be found in this [link](https://dexidp.io/docs/connectors/microsoft/).
{% endhint %}

Visit: [https://portal.azure.com/#home](https://portal.azure.com/#home) and select "Azure Active Directory"

<figure><img src="../../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

Select "App Registration".

<figure><img src="../../../.gitbook/assets/image (40) (4).png" alt=""><figcaption></figcaption></figure>

Click "New registration"

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

Create the application using:

* Name:&#x20;
  * TerrakubeDex
* Redirect URI:&#x20;
  * https://terrakube-api.sandbox.terrakube.io/dex/callback (Web)

<figure><img src="../../../.gitbook/assets/image (57) (1).png" alt=""><figcaption></figcaption></figure>

Copy the "Application Id" and "Tenant Id"

<figure><img src="../../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

You will also need to add the permission Directory.Read.All and ask a Azure administrator to approve it.

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Once the permission is approved it should look like this:

<figure><img src="../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

We need to generate a new secret for our application.

<figure><img src="../../../.gitbook/assets/image (58) (2).png" alt=""><figcaption></figcaption></figure>

Copy the "secret value"

<figure><img src="../../../.gitbook/assets/image (38) (3).png" alt=""><figcaption></figcaption></figure>

### Helm Deployment

Now we have all the information to deploy terrakube using the Azure Active Directory authentication.

Lets create a file called "terrakube.yaml" with the following content:

```
## Terrakube Security
security:
  useOpenLDAP: false
  adminGroup: "TERRAKUBE_ADMIN"
  dexClientId: "microsoft"
  dexClientScope: "email openid profile offline_access groups"
  dexIssuerUri: "https://terrakube-api.sandbox.terrakube.io/dex"

dex:
  config:
    issuer: https://terrakube-api.sandbox.terrakube.io/dex
    storage:
      type: memory
    oauth2:
      responseTypes: ["code", "token", "id_token"] 
      skipApprovalScreen: true
    web:
      allowedOrigins: ['*']
  
    staticClients:
    - id: microsoft
      redirectURIs:
      - 'https://terrakube-ui.sandbox.terrakube.io'
      - 'http://localhost:10001/login'
      - 'http://localhost:10000/login'
      - '/device/callback'
      name: 'microsoft'
      public: true

    connectors:
    - type: microsoft
      id: microsoft
      name: microsoft
      config:
        clientID: "<<CLIENT ID VALUE>>"
        clientSecret: "<<CLIENT ID SECRET>>"
        redirectURI: "https://terrakube-api.sandbox.terrakube.io/dex/callback"
        tenant: "<<TENANT ID>>"

## Ingress properties
ingress:
  useTls: true
  ui:
    enabled: true
    domain: "terrakube-ui.sandbox.terrakube.io"
    path: "/(.*)"
    pathType: "Prefix" 
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      cert-manager.io/cluster-issuer: letsencrypt
  api:
    enabled: true
    domain: "terrakube-api.sandbox.terrakube.io"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
      cert-manager.io/cluster-issuer: letsencrypt
  registry:
    enabled: true
    domain: "terrakube-reg.sandbox.terrakube.io"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
      cert-manager.io/cluster-issuer: letsencrypt
  dex:
    enabled: true
    path: "/dex/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
      cert-manager.io/cluster-issuer: letsencrypt

```

{% hint style="warning" %}
Dex configuration is using inside the redirect URIs http://localhost:10001/login and http://localhost:10000/login to support the terraform login protocol.
{% endhint %}

Run the installation

```bash
helm repo add terrakube-repo https://terrakube-io.github.io/terrakube-helm-chart
helm repo update
helm install terrakube terrakube-repo/terrakube --values terrakube.yaml -n terrakube
```

If the setup was correct we should be able to login using&#x20;

https://terrakube-ui.sandbox.terrakube.io

<figure><img src="../../../.gitbook/assets/image (1) (2) (3).png" alt=""><figcaption></figcaption></figure>

If we click the "login" button we should be able to see the Azure Active Directy login page.

<figure><img src="../../../.gitbook/assets/image (59) (1).png" alt=""><figcaption></figcaption></figure>

After a successfull login we should see Terrakube main page.

<figure><img src="../../../.gitbook/assets/image (66) (2).png" alt=""><figcaption></figcaption></figure>

Checking if all the resources are online using kubectl.

```
kubectl get pods -n terrakube
NAME                                  READY   STATUS    RESTARTS      AGE
terrakube-api-5c5f5d897d-n94kp        1/1     Running   1 (16m ago)   17m
terrakube-dex-865d9465c6-vsj7v        1/1     Running   0             13m
terrakube-executor-79bfb968c9-vrw2j   1/1     Running   0             17m
terrakube-minio-68cfdb95bc-6xrq9      1/1     Running   0             17m
terrakube-postgresql-0                1/1     Running   0             17m
terrakube-registry-f8dc58b79-648w9    1/1     Running   0             17m
terrakube-ui-7887b5c47-7kfw6          1/1     Running   0             17m
```
