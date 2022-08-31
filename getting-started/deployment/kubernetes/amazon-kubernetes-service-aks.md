# Amazon Cognito

{% hint style="warning" %}
AWS Cognito Authentication with Dex Connecor require Terrakube >= 2.6.0 and Helm Chart >= 2.0.0
{% endhint %}

### &#x20;Requirements

* AWS Cognito
* AWS S3 Bucket

For this example lets image that you will be using the following domains to deploy Terrakube.

* registry.terrakube.aws.com&#x20;
* ui.terrakube.aws.com&#x20;
* api.terrakube.aws.com

### Setup AWS Cognito Authentication

You need to complete the AWS Cognito authentication setup for Dex using the OIDC connector. You can found information in this [link](https://dexidp.io/docs/connectors/oidc/)

You need to create a new Cognito user pool

![](<../../../.gitbook/assets/image (1) (4).png>)

You can keep the default values

![](<../../../.gitbook/assets/image (36).png>)

Add the domain name to cognito

![](<../../../.gitbook/assets/image (9) (3).png>)

Once the user pool is created you will need to create a new application.

![](<../../../.gitbook/assets/image (5) (2).png>)

Update the application configuration and update the redirect URL configuration.

![](<../../../.gitbook/assets/image (4) (2).png>)

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
      issuer: https://api.terrakube.aws.com/dex #<<CHANGE_THIS>>
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"] 
        skipApprovalScreen: true
      web:
        allowedOrigins: ["*"]
  
      staticClients:
      - id: cognito
        redirectURIs:
        - 'https://ui.terrakube.aws.com' #<<CHANGE_THIS>>
        - 'http://localhost:3000'
        - 'http://localhost:10001/login'
        - 'http://localhost:10000/login'
        - '/device/callback'
        name: 'cognito'
        public: true

      connectors:
      - type: oidc
        id: cognito
        name: cognito
        config:
          issuer: "https://cognito-idp.XXXXX.amazonaws.com/XXXXXXX" #<<CHANGE_THIS>>
          clientID: "XXXX" #<<CHANGE_THIS>>
          clientSecret: "XXXXX" #<<CHANGE_THIS>>
          redirectURI: "https://api.terrakube.aws.com/dex/callback" #<<CHANGE_THIS>>
          scopes:
            - openid
            - email
            - profile
          insecureSkipEmailVerified: true
          insecureEnableGroups: true
          userNameKey: "cognito:username"
          claimMapping: 
            groups: "cognito:groups"
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
  dexClientId: "cognito"
  dexClientScope: "email openid profile offline_access groups"
  dexIssuerUri: "<<CHANGE_THIS>>" #The value should be like https://api.terrakube.azure.com/dex
  
## Terraform Storage
storage:
  aws:
    accessKey: "XXXXX" #<<CHANGE_THIS>>
    secretKey: "XXXXX" #<<CHANGE_THIS>>
    bucketName: "XXXXX" #<<CHANGE_THIS>>
    region: "XXXXX" #<<CHANGE_THIS>>

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
      issuer: https://api.terrakube.aws.com/dex #<<CHANGE_THIS>>
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"] 
        skipApprovalScreen: true
      web:
        allowedOrigins: ["*"]
  
      staticClients:
      - id: cognito
        redirectURIs:
        - 'https://ui.terrakube.aws.com' #<<CHANGE_THIS>>
        - 'http://localhost:3000'
        - 'http://localhost:10001/login'
        - 'http://localhost:10000/login'
        - '/device/callback'
        name: 'cognito'
        public: true

      connectors:
      - type: oidc
        id: cognito
        name: cognito
        config:
          issuer: "https://cognito-idp.XXXXX.amazonaws.com/XXXXXXX" #<<CHANGE_THIS>>
          clientID: "XXXX" #<<CHANGE_THIS>>
          clientSecret: "XXXXX" #<<CHANGE_THIS>>
          redirectURI: "https://api.terrakube.aws.com/dex/callback" #<<CHANGE_THIS>>
          scopes:
            - openid
            - email
            - profile
          insecureSkipEmailVerified: true
          insecureEnableGroups: true
          userNameKey: "cognito:username"
          claimMapping: 
            groups: "cognito:groups"

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
