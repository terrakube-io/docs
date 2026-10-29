# Ingress Configuration

The chart exposes the UI, API, registry and Dex through an ingress. Choose the controller with `ingress.controller` and set a domain for each service. The defaults are for NGINX with the example domains `*.minikube.net`; replace them.

| `ingress.controller` | Use for |
| --- | --- |
| `generic` (default) | Any Kubernetes ingress controller, such as NGINX. |
| `aws` | AWS Application Load Balancer (work in progress in the chart). |
| `gke` | Google Cloud Load Balancer, with managed certificates and Cloud Armor. |
| `gatewayapi` | Gateway API `HTTPRoute` resources. |

### NGINX (generic)

```yaml
ingress:
  controller: "generic"
  useTls: true
  ui:
    domain: "terrakube-ui.example.com"
    ingressClassName: "nginx"
    tlsSecretName: tls-secret-ui-terrakube
  api:
    domain: "terrakube-api.example.com"
    ingressClassName: "nginx"
    tlsSecretName: tls-secret-api-terrakube
  registry:
    domain: "terrakube-reg.example.com"
    ingressClassName: "nginx"
    tlsSecretName: tls-secret-reg-terrakube
  dex:
    enabled: true
    path: "/dex/"
```

Each service has its own `enabled`, `domain`, `path`, `ingressClassName`, `tlsSecretName` and `annotations`. The `executor` ingress is off by default, because the API reaches the executor inside the cluster.

### Other controllers

```yaml
# GKE
ingress:
  controller: "gke"
  gke:
    enabled: true
    managedCertificate:
      create: true
    backendConfig:
      create: true
  ui:
    domain: "terrakube-ui.example.com"
  api:
    domain: "terrakube-api.example.com"
  registry:
    domain: "terrakube-reg.example.com"
```

```yaml
# Gateway API
ingress:
  controller: "gatewayapi"
  gatewayapi:
    gateways:
      - name: "terrakube-gateway"
        namespace: "terrakube"
  ui:
    domain: "terrakube-ui.example.com"
  api:
    domain: "terrakube-api.example.com"
  registry:
    domain: "terrakube-reg.example.com"
```

For AWS ALB, GKE static IPs and Cloud Armor policies, see the [chart README](https://github.com/terrakube-io/terrakube-helm-chart#ingress-configuration) for every option.

### Things that must line up

* **Dex issuer.** `dex.config.issuer` must be the public API URL plus `/dex`, for example `https://terrakube-api.example.com/dex`, and the Dex client `redirectURIs` must include the UI URL. See [User Authentication (Dex)](user-authentication-dex.md).
* **Public discovery endpoints.** The API serves `/.well-known/openid-configuration` and `/.well-known/jwks` without authentication, and the chart keeps them public on every controller. Cloud providers use them to trust [dynamic provider credentials](../../user-guide/workspaces/dynamic-provider-credentials/README.md), so they must be reachable from the internet.
* **Authorization header.** The ingress must pass the `Authorization` header through to the API and registry.
* **Registry hostname.** Terraform and OpenTofu need HTTPS for the registry and remote state.

Install or upgrade with:

```bash
helm upgrade --install terrakube terrakube-repo/terrakube -n terrakube --values terrakube.yaml
```
