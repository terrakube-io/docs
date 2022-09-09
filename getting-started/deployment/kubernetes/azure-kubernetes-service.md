# Azure Active Directory

{% hint style="warning" %}
Azure Authentication with Dex Connecor require Terrakube >= 2.6.0 and Helm Chart >= 2.0.0
{% endhint %}

### &#x20;Requirements

* Azure Active Directory [here](https://developer.microsoft.com/en-us/microsoft-365/dev-program)
* Azure Storage Account

For this example lets image that you will be using the following domains to deploy Terrakube.

* registry.terrakube.azure.com&#x20;
* ui.terrakube.azure.com&#x20;
* api.terrakube.azure.com

### Setup Azure Authentication

You need to complete the Azure authentication setup for Dex. You can found information in this [link](https://dexidp.io/docs/connectors/microsoft/)

You need to go to your Azure and create a new Application

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

After the application is created you need to add the redirect URL.

<figure><img src="../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

You will also need to add the permission Directory.Read.All and ask a Azure administrator to approve the permission.

<figure><img src="../../../.gitbook/assets/image (1) (3).png" alt=""><figcaption></figcaption></figure>

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
      issuer: https://api.terrakube.azure.com/dex #<<CHANGE_THIS>>
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
        - 'https://ui.terrakube.azure.com' #<<CHANGE_THIS>>
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
          clientID: "XXXXXX" #<<CHANGE_THIS>>
          clientSecret: "XXXXXX" #<<CHANGE_THIS>>
          redirectURI: "https://api.terrakube.azure.com/dex/callback" #<<CHANGE_THIS>>
          tenant: "XXXXXX" #<<CHANGE_THIS>>
```

The firt step is to clone the repository.

```
git clone https://github.com/AzBuilder/terrakube-helm-chart.git
```

Replace _<\<CHANGE\_THIS>>_ with the real values, create the values.yaml file and run the helm install

```
## Global Name
name: "terrakube"

## Terrakube Security
security:
  adminGroup: "<<CHANGE_THIS>>" # The value should be a valida azure ad group (example: TERRAKUBE_ADMIN)
  patSecret: "<<CHANGE_THIS>>"  # Sample Key 32 characters z6QHX!y@Nep2QDT!53vgH43^PjRXyC3X 
  internalSecret: "<<CHANGE_THIS>>" # Sample Key 32 characters Kb^8cMerPNZV6hS!9!kcD*KuUPUBa^B3 
  dexClientId: "microsoft"
  dexClientScope: "email openid profile offline_access groups"
  dexIssuerUri: "<<CHANGE_THIS>>" #The value should be like https://api.terrakube.azure.com/dex
  
## Terraform Storage
storage:
  azure:
    storageAccountName: "XXXXXXX" # <<CHANGE_THIS>>
    storageAccountResourceGroup: "XXXXXXX" # <<CHANGE_THIS>>
    storageAccountAccessKey: "XXXXXXX" # <<CHANGE_THIS>>

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
      issuer: https://api.terrakube.azure.com/dex #<<CHANGE_THIS>>
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
        - 'https://ui.terrakube.azure.com' #<<CHANGE_THIS>>
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
          clientID: "XXXXXX" #<<CHANGE_THIS>>
          clientSecret: "XXXXXX" #<<CHANGE_THIS>>
          redirectURI: "https://api.terrakube.azure.com/dex/callback" #<<CHANGE_THIS>>
          tenant: "XXXXXX" #<<CHANGE_THIS>>

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
    domain: "ui.terrakube.azure.com"
    path: "/(.*)"
    pathType: "Prefix" 
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      cert-manager.io/cluster-issuer: letsencrypt
  api:
    enabled: true
    domain: "api.terrakube.azure.com"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
      cert-manager.io/cluster-issuer: letsencrypt
  registry:
    enabled: true
    domain: "registry.terrakube.azure.com"
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

Run the installation

```bash
helm install --debug --values ./values.yaml terrakube ./terrakube-helm-chart/ -n terrakube
```

{% hint style="warning" %}

{% endhint %}

{% hint style="warning" %}
For any question or feedback please open an issue in our [helm chart repository](https://github.com/AzBuilder/terrakube-helm-chart)
{% endhint %}
