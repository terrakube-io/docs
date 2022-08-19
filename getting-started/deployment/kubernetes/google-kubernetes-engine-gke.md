# Google Cloud Identity



{% hint style="warning" %}
Google Identity Authentication require Terrakube >= 2.6.0
{% endhint %}

To use Terrakube inside a GKE with PostgreSQL and GCP storage bucket for persisten storage please use the following steps:

### &#x20;Step 0 - Requirements

1. Google Cloud Identity [here](https://cloud.google.com/identity/docs/set-up-cloud-identity-admin#sign-up-for-cloud-identity-free)
2. GCP Storage Bucket [here](https://cloud.google.com/storage/docs/creating-buckets)
3. GKE with ngnix Ingress [here](https://cloud.google.com/kubernetes-engine/docs/concepts/ingress) and Let’s Encrypt configuration&#x20;
4. Setup Dex with Google Authentication [here](https://dexidp.io/docs/connectors/google/)
5. Public DNS

For this example lets image that you will be using the following domains to deploy Terrakube.

* registry.terrakube.gcp.com&#x20;
* ui.terrakube.gcp.com&#x20;
* api.terrakube.gcp.com

### Step 1 - Setup Google Authentication

You need to comple the Google authentication setup for Dex. You can found information in this [link](https://dexidp.io/docs/connectors/google/)

You need to go to your GCP projet and create a new OAuth Application you can follow this steps: firts select "APIs & Services => Credentials"

![](<../../../.gitbook/assets/image (4).png>)

Once inside the "Credentials" page, you will have to create a new OAuth Client

![](<../../../.gitbook/assets/image (3).png>)

The OAuth application should look like this with the redirect URL "[https://api.terrakube.gcp.com/dex/callback](https://api.terrakube.docker.com/dex/callback)"

![](<../../../.gitbook/assets/image (10).png>)

For Google authentication we need to get the GCP groups so you need to complete [this setup](https://dexidp.io/docs/connectors/google/#fetching-groups-from-google).

Include the Domain Wide Delegation inside the admin consol [https://admin.google.com/](https://admin.google.com/) for the OAuth application

![](<../../../.gitbook/assets/image (1).png>)

Using the following permission "[https://www.googleapis.com/auth/admin.directory.group.readonly](https://www.googleapis.com/auth/admin.directory.group.readonly)"

![](<../../../.gitbook/assets/image (8).png>)

You can now generate the JSON credentials file for your application, you will use this file later in the helm chart.

![](<../../../.gitbook/assets/image (6).png>)

Now you can create the DEX configuration, you will use this config later when deploying the helm chart.

```

    config:
      issuer: https://api.terrakube.gcp.com/dex
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"] 
      web:
        allowedOrigins: ['*']
  
      staticClients:
      - id: google
        redirectURIs:
        - 'https://ui.terrakube.gcp.com' # SHOULD BE YOUR REAL DOMAIN
        - 'http://localhost:10001/login' # USED WITH TERRAFORM LOGIN
        - 'http://localhost:10000/login' # USED WITH TERRAFORM LOGIN
        - '/device/callback' # USED WITH TERRAFORM LOGIN
        name: 'google'
        public: true

      connectors:
      - type: google
        id: google
        name: google
        config:
          clientID: "<<CLIENT ID FROM GOOGLE CLOUD>>"
          clientSecret: "<<CLIENT SECRET FROM GOOGLE CLOUD>>"
          redirectURI: "https://api.terrakube.gcp.com/dex/callback"
          serviceAccountFilePath: "/etc/gcp/secret/gcp-credentials"
          adminEmail: "superAdmin@demo.com"
```

### Step 2 - GCP Storage Bucket

You can follow [this](https://cloud.google.com/storage/docs/creating-buckets) to create the storage bucket.

### Step 3 - Deploy Terrakube with Helm.

Use the helm chart to deploy Terrakube in the cluster.

{% embed url="https://github.com/AzBuilder/terrakube-helm-chart" %}

{% hint style="warning" %}
Only  Helm chart version >=2.0.0 support dex authentication
{% endhint %}

The firt step is to clone the repository.

```
git clone https://github.com/AzBuilder/terrakube-helm-chart.git
```

You can use the following sample values and replace the require parameters

```
## Global Name
name: "terrakube"

## Terrakube Security
security:
  adminGroup: "<<CHANGE_THIS>>" # The value should be a gcp group (format: group_name@yourdomain.com example: terrakube_admin@terrakube.org)
  patSecret: "<<CHANGE_THIS>>"  # Sample Key 32 characters z6QHX!y@Nep2QDT!53vgH43^PjRXyC3X 
  internalSecret: "<<CHANGE_THIS>>" # Sample Key 32 characters Kb^8cMerPNZV6hS!9!kcD*KuUPUBa^B3 
  dexClientId: "google"
  dexClientScope: "email openid profile offline_access groups"
  dexIssuerUri: "<<CHANGE_THIS>>" #The value should be like https://api.terrakube.gcp.com/dex
  gcpCredentials: |
    ## GCP JSON CREDENTIALS for service account with API Scope https://www.googleapis.com/auth/admin.directory.group.readonly
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
    projectId: "<<CHANGE_THIS>>"
    bucketName: "<<CHANGE_THIS>>"
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
      issuer: https://api.terrakube.gcp.com/dex #<<CHANGE_THIS>>
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"] 
      web:
        allowedOrigins: ["*"]
  
      staticClients:
      - id: google
        redirectURIs:
        - 'https://ui.terrakube.gcp.com' #<<CHANGE_THIS>>
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
          redirectURI: "https://api.terrakube.gcp.com/dex/callback"
          serviceAccountFilePath: "/etc/gcp/secret/gcp-credentials" # GCP CREDENTIAL FILE WILL BE IN THIS PATH
          adminEmail: "<<CHANGE_THIS>>" 

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
    toolsRepository: "https://github.com/AzBuilder/terrakube-extensions"
    toolsBranch: "main"
    terraformStateType: "GcpTerraformStateImpl"
    terraformOutputType: "GcpTerraformOutputImpl"

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

```

{% hint style="warning" %}
For any question or feedback please open an issue in our [helm chart repository](https://github.com/AzBuilder/terrakube-helm-chart)
{% endhint %}
