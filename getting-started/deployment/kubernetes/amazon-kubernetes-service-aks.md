# Amazon Elastic Kubernetes Service (EKS)

To use Terrakube inside a AWS with Azure Active Directory authentication please follow these steps:

{% hint style="warning" %}
For now an Azure Active Directory is an authentication requirement, we are searching alternatives to include Amazon authentication, any help is welcome feel free to create an issue to discuss in [our repository](https://github.com/AzBuilder/terrakube-server)
{% endhint %}

### &#x20;Step 0 - Requirements

1. Azure Active Directory tenant for authentication. Get a free one [here](https://developer.microsoft.com/en-us/microsoft-365/dev-program)
2. Amazon RDS PostgreSQL
3. Amazon S3 Bucket
4. Amazon  EKS with Application Load Balancer
5. Public DNS

### Step 1 - Azure Active Directory Application

To register the Active Directory Application please use the following [terraform module.](https://github.com/AzBuilder/terraform-azurerm-terrakube-app-registration)

### Step 2 - Amazon RDS PostgreSQL

To setup the Amazon RDS PostgreSQL please refer to the following [documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP\_GettingStarted.CreatingConnecting.PostgreSQL.html).

### Step 3 - Amazon S3 Bucket

To setup an Amazon S3 bucket please refer to the following [documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html).

### Step 4 - Amazon EKS

To create the EKS cluster please refer to the following [documentation](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html), for the load balancer information please refer to this [document](https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html).

### Step 5 - Deploy Terrakube with Helm.

Use the helm chart to deploy Terrakube in the cluster.

{% embed url="https://github.com/AzBuilder/terrakube-helm-chart" %}

You can use the following sample values and replace the require parameters

```
## Global Name
name: "terrakube"

## Azure Active Directory Security
security:
  type: "AZURE" # This is the only value supported righ now
  azure:
    appIdURI: "XXX" # <--REPLACE WITH REAL VALUE
    appClientId: "XXX" # <--REPLACE WITH REAL VALUE
    appTenantId: "XXX" # <--REPLACE WITH REAL VALUE
    appSecret: "XXX" # <--REPLACE WITH REAL VALUE

## Terraform Storage
storage:
  aws:
    accessKey: "XXX" # <--REPLACE WITH REAL VALUE
    secretKey: "XXX" # <--REPLACE WITH REAL VALUE
    bucketName: "XXX" # <--REPLACE WITH REAL VALUE
    region: "XXX" # <--REPLACE WITH REAL VALUE

## API properties
api:
  enabled: true
  version: "2.4.1"
  replicaCount: "1"
  serviceType: "NodePort"
  resources: #Optional
    limits:
      cpu: 500m
      memory: 1024Mi
    requests:
      cpu: 200m
      memory: 256Mi
  properties:
    databaseType: "POSTGRESQL" # <--REPLACE WITH REAL VALUE Supported values "SQL_AZURE", "POSTGRESQL" or "MYSQL"
    databaseHostname: "terrakuaws.postgres.database.aws.com" # <--REPLACE WITH REAL VALUE
    databaseName: "databasename" # <--REPLACE WITH REAL VALUE
    databaseUser: "databaseuser" # <--REPLACE WITH REAL VALUE
    databasePassword: "XXX" # <--REPLACE WITH REAL VALUE

## Executor properties
executor:
  enabled: true
  version: "1.7.2"
  replicaCount: "1"
  serviceType: "NodePort"
  resources: #Optional
    limits:
      cpu: 1000m
      memory: 1024Mi
    requests:
      cpu: 500m
      memory: 256Mi
  properties:
    toolsRepository: "https://github.com/AzBuilder/terrakube-extensions" # Default extension repository
    toolsBranch: "main" #Default branch for extensions
    terraformStateType: "AwsTerraformStateImpl" 
    terraformOutputType: "AwsTerraformOutputImpl" 

## Registry properties
registry:
  enabled: true
  version: "2.4.1"
  replicaCount: "1"
  serviceType: "NodePort"
  resources: #Optional
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
  serviceType: "NodePort"
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
    domain: "terrakube-ui-dev.aws.dev" # <--REPLACE WITH REAL VALUE
    path: "/" 
    pathType: "Prefix" 
    annotations: # This annotations can change based on requirements. The followin is an example using EKS
      alb.ingress.kubernetes.io/actions.ssl-redirect: '{"Type": "redirect", "RedirectConfig": { "Protocol": "HTTPS", "Port": "443", "StatusCode": "HTTP_301"}}'
      alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:XXXXXX:certificate/XXXXXXXX # Change this for a real certiricate
      alb.ingress.kubernetes.io/group.name: alb-deployment
      alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/ssl-policy: ELBSecurityPolicy-TLS-1-1-2017-01
      alb.ingress.kubernetes.io/ssl-redirect: "443"
      external-dns.alpha.kubernetes.io/hostname: terrakube-ui-dev.aws.dev # <--REPLACE WITH REAL VALUE
      alb.ingress.kubernetes.io/target-type: ip
      kubernetes.io/ingress.class: alb
  api:
    enabled: true
    domain: "terrakube-api-dev.aws.dev" # <--REPLACE WITH REAL VALUE
    path: "/" 
    pathType: "Prefix" 
    annotations: # The followin is an example using EKS
      alb.ingress.kubernetes.io/actions.ssl-redirect: '{"Type": "redirect", "RedirectConfig": { "Protocol": "HTTPS", "Port": "443", "StatusCode": "HTTP_301"}}'
      alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:XXXXXX:certificate/XXXXXXXX # Replace with real certificate
      alb.ingress.kubernetes.io/group.name: alb-deployment
      alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/ssl-policy: ELBSecurityPolicy-TLS-1-1-2017-01
      alb.ingress.kubernetes.io/ssl-redirect: "443"
      external-dns.alpha.kubernetes.io/hostname: terrakube-api-dev.aws.dev # <--REPLACE WITH REAL VALUE
      alb.ingress.kubernetes.io/target-type: ip
      kubernetes.io/ingress.class: alb
  registry: 
    enabled: true
    domain: "terrakube-reg-dev.aws.dev" # <--REPLACE WITH REAL VALUE
    path: "/" 
    pathType: "Prefix" 
    annotations: # The followin is an example using EKS
      alb.ingress.kubernetes.io/actions.ssl-redirect: '{"Type": "redirect", "RedirectConfig": { "Protocol": "HTTPS", "Port": "443", "StatusCode": "HTTP_301"}}'
      alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:XXXXXX:certificate/XXXXXXXX # <--REPLACE WITH REAL VALUE
      alb.ingress.kubernetes.io/group.name: alb-deployment
      alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/ssl-policy: ELBSecurityPolicy-TLS-1-1-2017-01
      alb.ingress.kubernetes.io/ssl-redirect: "443"
      external-dns.alpha.kubernetes.io/hostname: terrakube-reg-dev.aws.dev # <--REPLACE WITH REAL VALUE
      alb.ingress.kubernetes.io/target-type: ip
      kubernetes.io/ingress.class: alb

```

{% hint style="warning" %}
For any question please open an issue in our [helm chart repository](https://github.com/AzBuilder/terrakube-helm-chart)
{% endhint %}
