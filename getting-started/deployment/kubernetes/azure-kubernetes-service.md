---
description: Setup Terrakube using SQL Azure and Azure Storage Account
---

# Azure Kubernetes Service (AKS)

To use Terrakube inside a AKS with SQL Azure and Azure Storage for persisten storage please use the following steps:

### &#x20;Step 0 - Requirements

1. Azure Active Directory tenant for authentication. Get a free one [here](https://developer.microsoft.com/en-us/microsoft-365/dev-program)
2. SQL Azure
3. Azure Storage account
4. Azure Kubernetes Service with Ngnix Ingress
5. Public DNS

### Step 1 - Azure Active Directory Application

To register the Active Directory Application please use the following [terraform module.](https://github.com/AzBuilder/terraform-azurerm-terrakube-app-registration)

### Step 2 - Azure SQL Azure

To create a SQL Azure Database please refer to the following Microsoft [guide](https://docs.microsoft.com/en-us/azure/azure-sql/database/single-database-create-quickstart?tabs=azure-portal).

### Step 3 - Create Azure Storage Account

To create the azure storage account please refer to the following [terraform module](https://github.com/AzBuilder/terraform-azurerm-terrakube-cloud-storage).

### Step 4 - Setup Let's Encrypt and Nginx Ingress

To setup an Ngnix Ingress Controller you can use the Microsoft guide:

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-basic?tabs=azure-cli" %}

To setup Azure Kubernetes Service with Lets Encrypt please refer to the followings guides:

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-tls" %}

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-static-ip" %}

### Step 5 - Deploy Terrakube with Helm.

Use the helm chart to deploy Terrakube in the cluster.

{% embed url="https://github.com/AzBuilder/terrakube-helm-chart" %}

You can use the following sample values and replace the require parameters

```
## Global Name
name: "terrakube"

## Azure Active Directory Security
security:
  type: "AZURE"
  azure:
    appIdURI: "api://Terrakube"
    appClientId: "XXXX" # <--REPLACE WITH REAL VALUE
    appTenantId: "XXXX" # <--REPLACE WITH REAL VALUE
    appSecret: "XXXX" # <--REPLACE WITH REAL VALUE

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
    databaseType: "SQL_AZURE"
    databaseHostname: "XXX" # <--REPLACE WITH REAL VALUE
    databaseName: "XXX" # <--REPLACE WITH REAL VALUE
    databaseUser: "XXX" # <--REPLACE WITH REAL VALUE
    databasePassword: "XXX" # <--REPLACE WITH REAL VALUE

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
    domain: "ui.terrakube.azure.com" # <--REPLACE WITH REAL VALUE
    path: "/(.*)"
    pathType: "Prefix" 
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      cert-manager.io/cluster-issuer: letsencrypt
  api:
    enabled: true
    domain: "api.terrakube.azure.com" # <--REPLACE WITH REAL VALUE
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
      cert-manager.io/cluster-issuer: letsencrypt
  registry:
    enabled: true
    domain: "registry.terrakube.azure.com" # <--REPLACE WITH REAL VALUE
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
      cert-manager.io/cluster-issuer: letsencrypt

```

{% hint style="warning" %}
For any question please open an issue in our [helm chart repository](https://github.com/AzBuilder/terrakube-helm-chart)
{% endhint %}
