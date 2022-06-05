# Docker Desktop

To use Terrakube inside Docker Desktop with H2 database and Azure Storage/Amazon S3 bucket for persisten storage please use the following steps:

### &#x20;Step 0 - Requirements

1. Azure Active Directory tenant for authentication. Get a free one [here](https://developer.microsoft.com/en-us/microsoft-365/dev-program).
2. Amazon S3 bucket or Azure Storage Account.
3. Docker Desktop with Ngnix Ingress.
4. Edit the network host file.

{% hint style="warning" %}
For now an Azure Active Directory is an authentication requirement, we are searching alternatives to include Amazon authentication, any help is welcome feel free to create an issue to discuss in [our repository](https://github.com/AzBuilder/terrakube-server)
{% endhint %}

### Step 1 - Azure Active Directory Application

To register the Active Directory Application please use the following [terraform module.](https://github.com/AzBuilder/terraform-azurerm-terrakube-app-registration)

### Step 2 - Docker Desktop

To learn how to install docker desktop please refer to the following documentation:

* [Windows](https://docs.docker.com/desktop/windows/install/)
* [Linux](https://docs.docker.com/desktop/linux/install/)
* [Mac](https://docs.docker.com/desktop/mac/install/)

### Step 3 - Ngnix Ingress for Docker Desktop

Please refer to the following documentation to install the ingress. [Link](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop)

### Step 4.1 - Deploy with Azure Storage Account

Use the helm chart to deploy Terrakube in the cluster.

{% embed url="https://github.com/AzBuilder/terrakube-helm-chart" %}

You can use the following sample values and replace the require parameters:

```
## Global Name
name: "terrakube"

## Azure Active Directory Security
security:
  type: "AZURE"
  azure:
    appIdURI: "api://Terrakube"
    appClientId: "XXX" # <--REPLACE WITH REAL VALUE
    appTenantId: "XXX" # <--REPLACE WITH REAL VALUE
    appSecret: "XXX" # <--REPLACE WITH REAL VALUE

## Terraform Storage
storage:
  azure:
    storageAccountName: "XXX" # <--REPLACE WITH REAL VALUE
    storageAccountResourceGroup: "XXX" # <--REPLACE WITH REAL VALUE
    storageAccountAccessKey: "XXX" # <--REPLACE WITH REAL VALUE

## API properties
api:
  enabled: true
  version: "2.4.1"
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 500m
      memory: 1024Mi
    requests:
      cpu: 200m
      memory: 256Mi
  properties:
    databaseType: "H2"

## Executor properties
executor:
  enabled: true
  version: "1.7.2"  
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 500m
      memory: 1024Mi
    requests:
      cpu: 200m
      memory: 256Mi
  properties:
    toolsRepository: "https://github.com/AzBuilder/terrakube-extensions"
    toolsBranch: "main"
    terraformStateType: "AzureTerraformStateImpl"
    terraformOutputType: "AzureTerraformOutputImpl"

## Registry properties
registry:
  enabled: true
  version: "2.4.1"
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 500m
      memory: 1024Mi
    requests:
      cpu: 200m
      memory: 256Mi

## UI Properties
ui:
  enabled: true
  version: "0.7.4"
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 500m
      memory: 512Mi
    requests:
      cpu: 200m
      memory: 256Mi

## Ingress properties
ingress:
  useTls: true
  ui:
    enabled: true
    domain: "ui.terrakube.docker.internal"
    path: "/(.*)"
    pathType: "Prefix" 
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
  api:
    enabled: true
    domain: "api.terrakube.docker.internal"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
  registry:
    enabled: true
    domain: "registry.terrakube.docker.internal"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"

```

### Step 4.2 - Deploy with Amazon S3 Buket

Use the helm chart to deploy Terrakube in the cluster.

{% embed url="https://github.com/AzBuilder/terrakube-helm-chart" %}

You can use the following sample values and replace the require parameters
