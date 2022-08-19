# Helm Chart

You can deploy Terrakube to any Kubernetes cluster using the helm chart available in the following repository:

[https://github.com/AzBuilder/terrakube-helm-chart](https://github.com/AzBuilder/terrakube-helm-chart)

#### Helm Properties

The helm chart support the following values:

| Key                                       | Required | Description                                                                                                     |
| ----------------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------- |
| name                                      | Yes      | Use "Terrakube"                                                                                                 |
| security.adminGroup                       | Yes      | Admin group inside Terrakube                                                                                    |
| security.patSecret                        | Yes      | 32 Character secret to sign personal access token                                                               |
| security.internalSecret                   | Yes      | 32 Character secret to sing internal                                                                            |
| security.dexClientId                      | Yes      | Based on Dex config file                                                                                        |
| security.dexClientScope                   | Yes      | Use "email openid profile offline\_access groups"                                                               |
| security.dexIssuerUri                     | Yes      | Should be "[https://apiDomain/dex](https://apidomain/dex)"                                                      |
| security.gcpCredentials                   | No       | JSON Credentials for Google Identity Authentication                                                             |
| storage.gcp.projectId                     | No       | GCP Project Id for the storage                                                                                  |
| storage.gcp.bucketName                    | No       | GCP Bucket name for the storage                                                                                 |
| storage.gcp.credentials                   | No       | GCP JSON Credentials for the storage                                                                            |
| storage.azure.storageAccountName          | No       | Azure storage account name                                                                                      |
| storage.azure.storageAccountResourceGroup | No       | Azure storage resource group                                                                                    |
| storage.azure.storageAccountAccessKey     | No       | Azure storage access key                                                                                        |
| dex.enabled                               | No       | Enable Dex component                                                                                            |
| dex.version                               | Yes      | Dex [version](https://github.com/dexidp/dex/releases)                                                           |
| dex.replicaCount                          | Yes      |                                                                                                                 |
| dex.serviceType                           | Yes      | ClusterIP/NodePort/LoadBalancer/ExternalName                                                                    |
| dex.resources                             | No       |                                                                                                                 |
| dex.properties.config                     | Yes      | Dex configuration file                                                                                          |
| dex.volumeMounts                          | No       |                                                                                                                 |
| dex.volumes                               | No       |                                                                                                                 |
| api.enabled                               | Yes      | true/false                                                                                                      |
| api.version                               | Yes      | Terrakube API version                                                                                           |
| api.replicaCount                          | Yes      |                                                                                                                 |
| api.serviceType                           | Yes      |                                                                                                                 |
| api.properties.databaseType               | Yes      | H2/SQL\_AZURE/POSTGRESQL/MYSQL                                                                                  |
| api.properties.databaseHostname           | No       |                                                                                                                 |
| api.properties.databaseName               | No       |                                                                                                                 |
| api.properties.databaseUser               | No       |                                                                                                                 |
| api.properties.databasePassword           | No       |                                                                                                                 |
| executor.enabled                          | Yes      | true/false                                                                                                      |
| executor.version                          | Yes      | Terrakube Executor version                                                                                      |
| executor.replicaCount                     | Yes      |                                                                                                                 |
| executor.serviceType                      | Yes      | ClusterIP/NodePort/LoadBalancer/ExternalName                                                                    |
| executor.properties.toolsRepository       | Yes      | Example: [https://github.com/AzBuilder/terrakube-extensions](https://github.com/AzBuilder/terrakube-extensions) |
| executor.properties.toolsBranch           | Yes      | Example: main                                                                                                   |
| registry.enabled                          | Yes      |                                                                                                                 |
| registry.version                          | Yes      |                                                                                                                 |
| registry.replicaCount                     | Yes      |                                                                                                                 |
| registry.serviceType                      | Yes      | ClusterIP/NodePort/LoadBalancer/ExternalName                                                                    |
| ui.enabled                                | Yes      | true/false                                                                                                      |
| ui.version                                | Yes      |                                                                                                                 |
| ui.replicaCount                           | Yes      |                                                                                                                 |
| ui.serviceType                            | Yes      | ClusterIP/NodePort/LoadBalancer/ExternalName                                                                    |
| ingress.ui.useTls                         | Yes      | true/false                                                                                                      |
| ingress.ui.enabled                        | Yes      | true/false                                                                                                      |
| ingress.ui.domain                         | Yes      |                                                                                                                 |
| ingress.ui.path                           | Yes      | ImplementationSpecific/Exact/Prefix                                                                             |
| ingress.ui.pathType                       | Yes      |                                                                                                                 |
| ingress.ui.annotations                    | Yes      | Ingress annotations                                                                                             |
| ingress.api.useTls                        | Yes      |                                                                                                                 |
| ingress.api.enabled                       | Yes      |                                                                                                                 |
| ingress.api.domain                        | Yes      |                                                                                                                 |
| ingress.api.path                          | Yes      |                                                                                                                 |
| ingress.api.pathType                      | Yes      | ImplementationSpecific/Exact/Prefix                                                                             |
| ingress.api.annotations                   | Yes      | Ingress annotations                                                                                             |
| ingress.registry.useTls                   | Yes      |                                                                                                                 |
| ingress.registry.enabled                  | Yes      |                                                                                                                 |
| ingress.registry.domain                   | Yes      |                                                                                                                 |
| ingress.registry.path                     | Yes      |                                                                                                                 |
| ingress.registry.pathType                 | Yes      | ImplementationSpecific/Exact/Prefix                                                                             |
| ingress.registry.annotations              | Yes      | Ingress annotations                                                                                             |
| ingress.dex.enabled                       | Yes      |                                                                                                                 |
| ingress.dex.domain                        | Yes      |                                                                                                                 |
| ingress.dex.path                          | Yes      |                                                                                                                 |
| ingress.dex.pathType                      | Yes      | ImplementationSpecific/Exact/Prefix                                                                             |
| ingress.dex.annontations                  | Yes      | Ingress annotations                                                                                             |

#### Node Affinity, NodeSelector, Taints and Tolerations.

The API, Dex, Registry, Executor and UI support using affinity, taints and tolerations. Use the following examples as reference:

**Example API.**

```
api:
  enabled: true
  version: "2.6.0"
  replicaCount: "1"
  serviceType: "ClusterIP"
  resources:
    limits:
      cpu: 500m
      memory: 1024Mi
    requests:
      cpu: 200m
      memory: 256Mi
  tolerations: # OPTIONAL
  - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoSchedule"
  nodeSelector: # OPTIONAL
    disktype: ssd
  affinity:  # OPTIONAL
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: name
            operator: In
            values:
            - terrakube
  properties:
    databaseType: "H2"
```

