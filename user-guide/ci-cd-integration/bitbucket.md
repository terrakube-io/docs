# Bitbucket

This example show how easy is to integrate Terrakube Jobs in Bitbucket pipelines.

### YAML Definition

Add the following snippet to the script section of your `bitbucket-pipelines.yml` file:

```
script:
  - pipe: terrakube-io/terrakube-pipe:1.0.0
    variables:
      LOGIN_ENDPOINT: "<string>" #optional Default: https://login.microsoftonline.com
      TERRAKUBE_TENANT_ID: "<string>"
      TERRAKUBE_APPLICATION_ID: "<string>"
      TERRAKUBE_APPLICATION_SECRET: "<string>"
      TERRAKUBE_APPLICATION_SCOPE: "<string>" #optional Default: api://Terrakube/.default
      TERRAKUBE_ORGANIZATION: "<string>"
      TERRAKUBE_WORKSPACE: "<string>"
      TERRAKUBE_TEMPLATE: "<string>"
      TERRAKUBE_ENDPOINT: "<string>"
      DEBUG: "<boolean>" # Optional Default: false
```

### Variables

| Variable                            | Usage                                                                                  |
| ----------------------------------- | -------------------------------------------------------------------------------------- |
| LOGIN\_ENDPOINT                     | Default values: [https://login.microsoftonline.com](https://login.microsoftonline.com) |
| TERRAKUBE\_TENANT\_ID (\*)          | Azure AD Application tenant ID                                                         |
| TERRAKUBE\_APPLICATION\_ID (\*)     | Azure AD Application tenant ID                                                         |
| TERRAKUBE\_APPLICATION\_SECRET (\*) | Azure AD Application tenant ID                                                         |
| TERRAKUBE\_APPLICATION\_SCOPE       | Default value: api://Terrakube/.default                                                |
| TERRAKUBE\_ORGANIZATION (\*)        | Terrakube organization name                                                            |
| TERRAKUBE\_WORKSPACE (\*)           | Terrakube workspace name                                                               |
| TERRAKUBE\_TEMPLATE (\*)            | Terrakube template name                                                                |
| TERRAKUBE\_ENDPOINT (\*)            | Terrakbue api endpoint                                                                 |

_(\*) = required variable._

### Examples

Basic example:

```
script:
  - pipe: docker://terrakube-io/terrakube-pipe:1.0.0
    variables:
      TERRAKUBE_TENANT_ID: "36857254-c824-409f-96f5-d3f2de37b016"
      TERRAKUBE_APPLICATION_ID: "36857254-c824-409f-96f5-d3f2de37b016"
      TERRAKUBE_APPLICATION_SECRET: "SuperSecret"
      TERRAKUBE_ORGANIZATION: "terrakube"
      TERRAKUBE_WORKSPACE: "bitbucket"
      TERRAKUBE_TEMPLATE: "vulnerability-snyk"
      TERRAKUBE_ENDPOINT: "https://terrakube.interal/service"
```

Advanced example:

```
script:
  - pipe: docker://terrakube-io/terrakube-pipe:1.0.0
    variables:
      LOGIN_ENDPOINT: "https://login.microsoftonline.com"
      TERRAKUBE_TENANT_ID: "36857254-c824-409f-96f5-d3f2de37b016"
      TERRAKUBE_APPLICATION_ID: "36857254-c824-409f-96f5-d3f2de37b016"
      TERRAKUBE_APPLICATION_SECRET: "SuperSecret"
      TERRAKUBE_APPLICATION_SCOPE: "api://TerrakubeApp/.default"
      TERRAKUBE_ORGANIZATION: "terrakube"
      TERRAKUBE_WORKSPACE: "bitbucket"
      TERRAKUBE_TEMPLATE: "vulnerability-snyk"
      TERRAKUBE_ENDPOINT: "https://terrakube.interal/service"
      DEBUG: "true"
```

For more information about this pipe please refer to the following [repository](https://github.com/terrakube-io/terrakube-pipe-bitbucket).
