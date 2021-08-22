---
description: Setup AzBuilder using SQL Azure and Azure Storage Account
---

# Azure Kubernetes Service

Step 1 - Create Kubernetes Secrets

You can use the following yaml file to create all the secrets but you need to replace **XXXXX** with correct values.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azbuilder-api-secrets
type: Opaque
stringData:
  ApiDataSourceType: 'SQL_AZURE'
  SqlAzureServer: 'XXXXX.database.windows.net'
  SqlAzureDatabase: 'XXXXX'
  SqlAzureUser: 'XXXXX'
  SqlAzurePassword: 'XXXXX'
  AzureAdAppId: 'XXXXX'
  AzureAdApiIdUri: 'api://azbuilder'
---
apiVersion: v1
kind: Secret
metadata:
  name: azbuilder-api-job-secrets
type: Opaque
stringData:
  AzBuilderApiUrl: 'http://aks-azbuilder-service:8080'
  AzureAdAppClientId: 'XXXXX'
  AzureAdAppClientSecret: 'XXXXX'
  AzureAdAppTenantId: 'XXXXX'
  AzureAdAppScope: 'api://azbuilder/.default'
  AzBuilderExecutorUrl: 'http://aks-azbuilder-executor-service:8090/api/v1/terraform-rs'
---
apiVersion: v1
kind: Secret
metadata:
  name: azbuilder-executor-secrets
type: Opaque
stringData:
  TerraformStateType: 'AzureTerraformStateImpl'
  AzureTerraformStateResourceGroup: 'XXXXX'
  AzureTerraformStateStorageAccountName: 'XXXXX'
  AzureTerraformStateStorageContainerName: 'XXXXX'
  AzureTerraformStateStorageAccessKey: 'XXXXX'
  TerraformOutputType: 'AzureTerraformOutputImpl'
  AzureTerraformOutputAccountName: 'XXXXX'
  AzureTerraformOutputAccountKey: 'XXXXX'
  AzBuilderApiUrl: 'http://aks-azbuilder-service:8080'
  AzureAdAppClientId: 'XXXXX'
  AzureAdAppClientSecret: 'XXXXX'
  AzureAdAppTenantId: 'XXXXX'
  AzureAdAppScope: 'api://azbuilder/.default'
  ExecutorFlagBatch: 'false'
  ExecutorFlagDisableAcknowledge: 'false'
---
apiVersion: v1
kind: Secret
metadata:
  name: azbuilder-open-registry-secrets
type: Opaque
stringData:
  AzBuilderRegistry: 'XXXXX'
  AzureAccountName: 'XXXXX'
  AzureAccountKey: 'XXXXX'
  AzBuilderApiUrl: 'http://aks-azbuilder-service:8080'
  AzureAdAppClientId: 'XXXXX'
  AzureAdAppClientSecret: 'XXXXX'
  AzureAdAppTenantId: 'XXXXX'
  AzureAdAppScope: 'api://azbuilder/.default'
```

### Step 2 - Deploy AzBuilder Platform

Use the following yaml to deploy the service to the Azure Kubernetes Service

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azbuilder-api
  labels:
    app: azbuilder-api
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azbuilder-api
  template:
    metadata:
      labels:
        app: azbuilder-api
    spec:
      containers:
      - name: azbuilder-api
        image: azbuilder/api-server:latest
        ports:
        - containerPort: 8080
        envFrom:
        - secretRef:
            name: azbuilder-api-secrets
---
apiVersion: v1
kind: Service
metadata:
  name: aks-azbuilder-service  
spec:
  type: ClusterIP
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    app: azbuilder-api
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azbuilder-api-job
  labels:
    app: azbuilder-api-job
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azbuilder-api-job
  template:
    metadata:
      labels:
        app: azbuilder-api-job
    spec:
      containers:
      - name: azbuilder-api
        image: azbuilder/api-job:latest
        ports:
        - containerPort: 8085
        envFrom:
        - secretRef:
            name: azbuilder-api-job-secrets
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azbuilder-api-executor
  labels:
    app: azbuilder-api-executor
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azbuilder-api-executor
  template:
    metadata:
      labels:
        app: azbuilder-api-executor
    spec:
      containers:
      - name: azbuilder-api-executor
        image: azbuilder/executor:latest
        ports:
        - containerPort: 8090
        envFrom:
        - secretRef:
            name: azbuilder-executor-secrets
---
apiVersion: v1
kind: Service
metadata:
  name: aks-azbuilder-executor-service  
spec:
  type: ClusterIP
  ports:
  - port: 8090
    targetPort: 8090
  selector:
    app: azbuilder-api-executor
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azbuilder-open-registry
  labels:
    app: azbuilder-open-registry
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azbuilder-open-registry
  template:
    metadata:
      labels:
        app: azbuilder-open-registry
    spec:
      containers:
      - name: azbuilder-api
        image: azbuilder/open-registry:latest
        ports:
        - containerPort: 8075
        envFrom:
        - secretRef:
            name: azbuilder-open-registry-secrets
---
apiVersion: v1
kind: Service
metadata:
  name: aks-azbuilder-open-registry-service  
spec:
  type: ClusterIP
  ports:
  - port: 8075
    targetPort: 8075
  selector:
    app: azbuilder-open-registry
```

### Step 3 - Setup Let's Encrypt with AKS

To setup Azure Kubernetes Service with Lets Encrypt please refer to the followings guides:

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-tls" %}

{% embed url="https://docs.microsoft.com/en-us/azure/aks/ingress-static-ip" %}

### Step 4 - Ingress Controller

To setup the NGINX ingress controller use the following yaml and replace the **XXXX** with the correct values.

_Keep in mind that if you want to use the Open-Registry you will need to use two domains._

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: azbuilder-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/enable-cors: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$2
    nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
    - XXXXX
    secretName: tls-secret
  rules:
  - host:  XXXXX
    http:
      paths:
      - path: /azbuilder(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-azbuilder-service
            port:
              number: 8080
      - path: /open-registry(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-azbuilder-open-registry-service
            port:
              number: 8075
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: open-registry-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/use-regex: "true"
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
    - XXXXX
    secretName: tls-secret-registry
  rules:
  - host:  XXXXX
    http:
      paths:
      - path: /(.*)
        pathType: Prefix
        backend:
          service:
            name: aks-azbuilder-open-registry-service
            port:
              number: 8075
```

