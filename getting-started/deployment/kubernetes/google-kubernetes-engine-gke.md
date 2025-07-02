# Google Cloud Identity

{% hint style="warning" %}
Google Identity Authentication with Dex connector require Terrakube >= 2.6.0 and Helm Chart >= 2.0.0
{% endhint %}

### &#x20;Requirements

* Google Cloud Identity [here](https://cloud.google.com/identity/docs/set-up-cloud-identity-admin#sign-up-for-cloud-identity-free)
* Gooble Storage Bucket

For this example lets image that you will be using the following domains to deploy Terrakube.

* registry.terrakube.gcp.com&#x20;
* ui.terrakube.gcp.com&#x20;
* api.terrakube.gcp.com

### Setup Google Authentication

You need to complete the Google authentication setup for Dex. You can found information in this [link](https://dexidp.io/docs/connectors/google/)

You need to go to your GCP projet and create a new OAuth Application you can follow this steps: firts select "APIs & Services => Credentials"

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

Once inside the "Credentials" page, you will have to create a new OAuth Client

<figure><img src="../../../.gitbook/assets/image (3) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

The OAuth application should look like this with the redirect URL "[https://api.terrakube.gcp.com/dex/callback](https://api.terrakube.docker.com/dex/callback)"

<figure><img src="../../../.gitbook/assets/image (10) (2).png" alt=""><figcaption></figcaption></figure>

For Google authentication we need to get the GCP groups so you need to complete [this setup](https://dexidp.io/docs/connectors/google/#fetching-groups-from-google).

Include the Domain Wide Delegation inside the admin consol [https://admin.google.com/](https://admin.google.com/) for the OAuth application

<figure><img src="../../../.gitbook/assets/image (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

Using the following permission "[https://www.googleapis.com/auth/admin.directory.group.readonly](https://www.googleapis.com/auth/admin.directory.group.readonly)"

<figure><img src="../../../.gitbook/assets/image (8) (3).png" alt=""><figcaption></figcaption></figure>

You can now generate the JSON credentials file for your application, you will use this file later in the helm chart.

<figure><img src="../../../.gitbook/assets/image (6) (1) (2).png" alt=""><figcaption></figcaption></figure>

Now you can create the DEX configuration, you will use this config later when deploying the helm chart.

```

## Dex
dex:
  enabled: true
  version: "v2.32.0"
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 512m
      memory: 256Mi
    requests:
      cpu: 256m
      memory: 128Mi
  properties:
    config:
      issuer: https://terrakube-api.yourdomain.com/dex #<<CHANGE_THIS>>
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"] 
      web:
        allowedOrigins: ["*"]
  
      staticClients:
      - id: google
        redirectURIs:
        - 'https://terrakube-ui.yourdomain.com' #<<CHANGE_THIS>>
        - 'http://localhost:3000'
        - 'http://localhost:10001/login'
        - 'http://localhost:10000/login'
        - '/device/callback'
        name: 'google'
        public: true

      connectors:
      - type: google
        id: google
        name: google
        config:
          clientID: "<<CHANGE_THIS>>"
          clientSecret: "<<CHANGE_THIS>>"
          redirectURI: "https://terrakube-api.yourdomain.com/dex/callback"
          serviceAccountFilePath: "/etc/gcp/secret/gcp-credentials" # GCP CREDENTIAL FILE WILL BE IN THIS PATH
          adminEmail: "<<CHANGE_THIS>>"
```

The firt step is to clone the repository.

```
git clone https://github.com/terrakube-io/terrakube-helm-chart.git
```

Replace _<\<CHANGE\_THIS>>_ with the real values, create the values.yaml file and run the helm install

<pre><code>## Global Name
name: "terrakube"

## Terrakube Security
security:
  adminGroup: "&#x3C;&#x3C;CHANGE_THIS>>" # The value should be a gcp group (format: group_name@yourdomain.com example: terrakube_admin@terrakube.io)
  patSecret: "&#x3C;&#x3C;CHANGE_THIS>>"  # Sample Key 32 characters z6QHX!y@Nep2QDT!53vgH43^PjRXyC3X 
  internalSecret: "&#x3C;&#x3C;CHANGE_THIS>>" # Sample Key 32 characters Kb^8cMerPNZV6hS!9!kcD*KuUPUBa^B3 
  dexClientId: "google"
  dexClientScope: "email openid profile offline_access groups"
  dexIssuerUri: "&#x3C;&#x3C;CHANGE_THIS>>" #The value should be like https://api.terrakube.gcp.com/dex
<strong>  gcpCredentials: |
</strong>    ## GCP JSON CREDENTIALS for service account with API Scope https://www.googleapis.com/auth/admin.directory.group.readonly
    {
      "type": "service_account",
      "project_id": "",
      "private_key_id": "",
      "private_key": "",
      "client_email": "",
      "client_id": "",
      "auth_uri": "",
      "token_uri": "",
      "auth_provider_x509_cert_url": "",
      "client_x509_cert_url": ""
    } 


## Terraform Storage
storage:
  gcp:
    projectId: "&#x3C;&#x3C;CHANGE_THIS>>"
    bucketName: "&#x3C;&#x3C;CHANGE_THIS>>"
    credentials: |
      ## GCP JSON CREDENTIALS for service account with access to write to the storage bucket
      {
        "type": "service_account",
        "project_id": "",
        "private_key_id": "",
        "private_key": "",
        "client_email": "",
        "client_id": "",
        "auth_uri": "",
        "token_uri": "",
        "auth_provider_x509_cert_url": "",
        "client_x509_cert_url": ""
      } 


## Dex
dex:
  enabled: true
  version: "v2.32.0"
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 512m
      memory: 256Mi
    requests:
      cpu: 256m
      memory: 128Mi
  properties:
    config:
      issuer: https://api.terrakube.gcp.com/dex #&#x3C;&#x3C;CHANGE_THIS>>
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"] 
      web:
        allowedOrigins: ["*"]
  
      staticClients:
      - id: google
        redirectURIs:
        - 'https://ui.terrakube.gcp.com' #&#x3C;&#x3C;CHANGE_THIS>>
        - 'http://localhost:10001/login'
        - 'http://localhost:10000/login'
        - '/device/callback'
        name: 'google'
        public: true

      connectors:
      - type: google
        id: google
        name: google
        config:
          clientID: "&#x3C;&#x3C;CHANGE_THIS>>"
          clientSecret: "&#x3C;&#x3C;CHANGE_THIS>>"
          redirectURI: "https://api.terrakube.gcp.com/dex/callback"
          serviceAccountFilePath: "/etc/gcp/secret/gcp-credentials" # GCP CREDENTIAL FILE WILL BE IN THIS PATH
          adminEmail: "&#x3C;&#x3C;CHANGE_THIS>>" 

## API properties
api:
  enabled: true
  version: "2.6.0"
  replicaCount: "1"
  serviceType: "ClusterIP"
  properties:
    databaseType: "H2"

## Executor properties
executor:
  enabled: true
  version: "2.6.0"  
  replicaCount: "1"
  serviceType: "ClusterIP"
  properties:
    toolsRepository: "https://github.com/terrakube-io/terrakube-extensions"
    toolsBranch: "main"

## Registry properties
registry:
  enabled: true
  version: "2.6.0"
  replicaCount: "1"
  serviceType: "ClusterIP"

## UI Properties
ui:
  enabled: true
  version: "2.6.0"
  replicaCount: "1"
  serviceType: "ClusterIP"

## Ingress properties
ingress:
  useTls: true
  ui:
    enabled: true
    domain: "terrakube-ui.yourdomain.com"
    path: "/(.*)"
    pathType: "Prefix" 
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      cert-manager.io/cluster-issuer: letsencrypt
  api:
    enabled: true
    domain: "terrakube-api.yourdomain.com"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
      cert-manager.io/cluster-issuer: letsencrypt
  registry:
    enabled: true
    domain: "terrakube-reg.yourdomain.com"
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

</code></pre>

Run the installation

```bash
helm install --debug --values ./values.yaml terrakube ./terrakube-helm-chart/ -n terrakube
```

{% hint style="warning" %}

{% endhint %}

{% hint style="warning" %}
For any question or feedback please open an issue in our [helm chart repository](https://github.com/terrakube-io/terrakube-helm-chart)
{% endhint %}
