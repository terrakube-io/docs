# Docker Desktop

### Requirements <a href="#requirements" id="requirements"></a>

To run Terrakube in Docker Desktop you wil need the following:

* Create Github [Organization](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/creating-a-new-organization-from-scratch)
* Create Github [Teams](https://docs.github.com/en/github-ae@latest/admin/user-management/managing-organizations-in-your-enterprise/creating-teams) and add some [members](https://docs.github.com/en/github-ae@latest/admin/user-management/managing-organizations-in-your-enterprise/adding-people-to-teams)
* Create a team called TERRAKUBE\_ADMIN and add the members
* Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* Nginx Ingress for [Docker Desktop](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop)
* Azure Storage Account/AWS S3 Bucket/GCP Storage Bucket

> To get more information about the Dex Configuration for Github you can check this [link](https://dexidp.io/docs/connectors/github/)

### Setup <a href="#setup" id="setup"></a>

* Create a new Github Oauth application with the authorization callback URL "[http://host.docker.internal/dex/callback](http://host.docker.internal/dex/callback)" in this [link](https://github.com/settings/developers)
* Copy the values for Client Id and Client Secret
* Update the HOSTS file adding the following

```
# Linux Path /etc/hosts
# Windows Path c:\Windows\System32\Drivers\etc\hosts


127.0.0.1 ui.terrakube.docker.com
127.0.0.1 registry.terrakube.docker.com
```

### YAML Example <a href="#yaml-example" id="yaml-example"></a>

Replace _**<\<CHANGE\_THIS>>**_ with the real values, create the values.yaml file and run the helm install

```
## Global Name
name: "terrakube"

## Terrakube Security
security:
  adminGroup: "<<CHANGE_THIS>>" # This should be your Github team the format is OrganizationName:TeamName (Example: MyOrg:TERRAKUBE_ADMIN)
  patSecret: "<<CHANGE_THIS>>"  # Sample Key 32 characters z6QHX!y@Nep2QDT!53vgH43^PjRXyC3X 
  internalSecret: "<<CHANGE_THIS>>" # Sample Key 32 characters Kb^8cMerPNZV6hS!9!kcD*KuUPUBa^B3 
  dexClientId: "github"
  dexClientScope: "email openid profile offline_access groups"
  dexIssuerUri: "http://host.docker.internal/dex" # Change for your real domain

## Terraform Storage
storage:
  # SELECT THE TYPE OF STORAGE THAT YOU WANT TO USE AND REPLACE THE VALUES
  
  #azure:
  #  storageAccountName: "<<CHANGE_THIS>>"
  #  storageAccountResourceGroup: "<<CHANGE_THIS>>"
  #  storageAccountAccessKey: "<<CHANGE_THIS>>"
  #aws:
  #  accessKey: "<<CHANGE_THIS>>"
  #  secretKey: "<<CHANGE_THIS>>"
  #  bucketName: "<<CHANGE_THIS>>"
  #  region: "<<CHANGE_THIS>>"
  #gcp:
  #  projectId: "<<CHANGE_THIS>>"
  #  bucketName: "<<CHANGE_THIS>>"
  #  credentials: |
  #    ## GCP JSON CREDENTIALS for service account with access to read/write to the storage bucket
  #    {
  #      "type": "service_account",
  #      "project_id": "",
  #      "private_key_id": "",
  #      "private_key": "",
  #      "client_email": "",
  #      "client_id": "",
  #      "auth_uri": "",
  #      "token_uri": "",
  #      "auth_provider_x509_cert_url": "",
  #      "client_x509_cert_url": ""
  #    } 

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
      issuer: http://host.docker.internal/dex
      storage:
        type: memory
      oauth2:
        responseTypes: ["code", "token", "id_token"] 
        skipApprovalScreen: true
      web:
        allowedOrigins: ["*"]
  
      staticClients:
      - id: github
        redirectURIs:
        - 'http://ui.terrakube.docker.com'
        - 'http://localhost:10001/login'
        - 'http://localhost:10000/login'
        - '/device/callback'
        name: 'github'
        public: true

      connectors:
      - type: github
        id: github
        name: gitHub
        config:
          clientID: "<<CHANGE_THIS>>" 
          clientSecret: "<<CHANGE_THIS>>"
          redirectURI: "http://host.docker.internal/dex/callback"
          loadAllGroups: true

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
  useTls: false
  ui:
    enabled: true
    domain: "ui.terrakube.docker.com"
    path: "/(.*)"
    pathType: "Prefix" 
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      cert-manager.io/cluster-issuer: letsencrypt
  api:
    enabled: true
    domain: "host.docker.internal"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
  registry:
    enabled: true
    domain: "registry.terrakube.docker.com"
    path: "/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
  dex:
    enabled: true
    path: "/dex/(.*)"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
```

Run the installation

```bash
helm install --debug --values ./values.yaml terrakube ./terrakube-helm-chart/ -n terrakube
```

{% hint style="warning" %}
For any question please open an issue in our [helm chart repository](https://github.com/terrakube-io/terrakube-helm-chart)
{% endhint %}
