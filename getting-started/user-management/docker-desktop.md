# GitHub (Docker Desktop)

Sign in to Terrakube with GitHub accounts, using a GitHub organization's teams as Terrakube groups. This example runs on the Kubernetes cluster in Docker Desktop but the Dex configuration is the same on any cluster.

### Requirements

* A GitHub organization with a team for your Terrakube administrators (for example `TERRAKUBE_ADMIN`) and members added to it.
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) with Kubernetes enabled and the [NGINX ingress controller](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop).
* A storage backend and the Terrakube Helm repository; see [Helm Chart](../deployment/helm-chart.md).

### Create a GitHub OAuth app

In GitHub, create an OAuth app (**Settings → Developer settings → OAuth Apps**) with the authorization callback URL `https://<your API domain>/dex/callback`. Copy the client ID and generate a client secret.

### Values

Replace the `<<CHANGE_THIS>>` values. GitHub groups are written `OrganizationName:TeamName`.

```yaml
security:
  useOpenLDAP: false
  adminGroup: "MyOrg:TERRAKUBE_ADMIN"
  patSecret: "<<CHANGE_THIS>>"       # 32 characters, see Token Security
  internalSecret: "<<CHANGE_THIS>>"  # 32 characters, see Token Security
  dexClientId: "github"
  dexClientScope: "email openid profile offline_access groups"

dex:
  config:
    issuer: https://terrakube-api.example.com/dex
    storage:
      type: memory
    oauth2:
      responseTypes: ["code", "token", "id_token"]
    web:
      allowedOrigins: ['*']
    staticClients:
    - id: github
      name: github
      public: true
      redirectURIs:
      - 'https://terrakube-ui.example.com'
      - 'http://localhost:10001/login'
      - 'http://localhost:10000/login'
      - '/device/callback'
    connectors:
    - type: github
      id: github
      name: GitHub
      config:
        clientID: "<<CHANGE_THIS>>"
        clientSecret: "<<CHANGE_THIS>>"
        redirectURI: "https://terrakube-api.example.com/dex/callback"
        loadAllGroups: true

ingress:
  ui:
    domain: "terrakube-ui.example.com"
  api:
    domain: "terrakube-api.example.com"
  registry:
    domain: "terrakube-reg.example.com"
```

Add your [storage backend](../deployment/storage-backend/README.md) and [database](../deployment/database-backend/README.md), then install:

```bash
helm install terrakube terrakube-repo/terrakube -n terrakube --values values.yaml
```

Sign in at the UI domain. Members of your administrator team can create organizations; give other teams access with [Team Management](../../user-guide/organizations/team-management.md), naming each Terrakube team after the GitHub team in the same `OrganizationName:TeamName` form.

For more on the connector, see the [Dex GitHub connector](https://dexidp.io/docs/connectors/github/) and [User Authentication (Dex)](../deployment/user-authentication-dex.md).
