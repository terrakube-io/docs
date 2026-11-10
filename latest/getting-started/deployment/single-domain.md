# Single-Domain Deployment

{% hint style="warning" %}
**Experimental in 2.35.** Single-domain mode is opt-in. Installations that do not set `TerrakubePublicUrl` behave exactly as before.
{% endhint %}

By default Terrakube uses one hostname per component: UI, API, registry and Dex. In single-domain
mode all of them are served from one hostname, for example `https://terrakube.example.com`, with
routing by path. You need one DNS record, one TLS certificate and one ingress.

## Layout

| Path on the shared host | Component |
| --- | --- |
| `/.well-known/terraform.json`, `/.well-known/openid-configuration`, `/.well-known/jwks` | API |
| `/api/private-registry/...` | Registry |
| `/api/...` | API |
| `/webhook/...`, `/callback/...` | API (webhooks and OAuth redirects registered before a migration) |
| `/access-token/...` | API (team tokens, as called by released Terraform provider and CLI versions) |
| `/dex/...` | Dex |
| everything else | UI |

[`docs/single-domain/route-table.yaml`](https://github.com/terrakube-io/terrakube/blob/main/docs/single-domain/route-table.yaml) in the Terrakube repository holds the same rules in a machine-readable form.

An API path is the legacy path with an `/api` prefix: `/tfstate/v1/...` becomes
`/api/tfstate/v1/...`, and `/graphql/api/v1` becomes `/api/graphql/api/v1`. JSON:API at `/api/v1/...`
keeps its path. A registry path is the legacy path with an `/api/private-registry` prefix. The API
and the registry serve these prefixed paths only when `TerrakubePublicUrl` is set, with the same
authorization as the legacy paths. Split-domain installations serve the legacy paths alone, so
ingress or WAF rules that match them keep covering every endpoint. If you restrict paths at the
proxy, add the `/api/...` equivalents of those rules when you turn single-domain mode on.

The Terraform and OpenTofu CLIs find every service through `/.well-known/terraform.json`, so they
only need the hostname.

## Configuration

Set one value, `TerrakubePublicUrl`, on the API, the registry and the executor. It must be an
absolute URL with no path, query or trailing slash. The UI reads the same value as
`REACT_APP_TERRAKUBE_PUBLIC_URL` in `env-config.js`.

| Setting | When unset, it is derived from the public URL as |
| --- | --- |
| `TerrakubeHostname` (API) | none: generated API URLs use `<public>/api`. An explicit value still wins (see [Migrating](#migrating-an-existing-installation)). |
| `TerrakubeUiURL` | `<public>` |
| `DexIssuerUri`, `AppIssuerUri` | `<public>/dex` |
| `TerraformLoginApiUrl` | `<public>/api` |
| `AzBuilderRegistry` (registry) | `<public>/api/private-registry` |
| `TerrakubeRegistryDomain` (executor) | the public URL's host |
| `DynamicCredentialHostname` | `TerrakubeHostname` if set, otherwise the public URL's host |

Explicitly set variables always win over derived values. Keep these on internal addresses:
`AzBuilderApiUrl` (registry, executor), `TerrakubeApiUrl` (executor) and `AzBuilderExecutorUrl`
(API). Server-to-server traffic does not need to go through the shared ingress.

Executors and agents that run outside the cluster can use the shared host instead: set
`AzBuilderApiUrl` and `TerrakubeApiUrl` to `<public>/api`. Run output and state links that the
executor stores for users always use `<public>/api` in single-domain mode.

For the UI, set these in `env-config.js`:

```js
REACT_APP_TERRAKUBE_PUBLIC_URL: "https://terrakube.example.com",
REACT_APP_TERRAKUBE_API_URL: "https://terrakube.example.com/api/v1/",
REACT_APP_AUTHORITY: "https://terrakube.example.com/dex",
REACT_APP_REGISTRY_URI: "https://terrakube.example.com/api/private-registry",
REACT_APP_REDIRECT_URI: "https://terrakube.example.com",
```

Point the Dex `issuer` at `<public>/dex`, and add `<public>` (and, with the CLI login broker,
`<public>/api/oauth/callback`) to the static client's redirect URIs.

To keep the registry on its own hostname, set `AzBuilderRegistry` on the registry and the API.
The API then leaves the registry out of its discovery document, and module sources keep using the
registry hostname.

## Proxy requirements

- Send the paths above to the components unchanged. Do not strip any prefix.
- Under `/api/` the API and the registry reject paths that contain `;`, `//`, `\` or encoded dots,
  slashes and backslashes with `400`.
- Allow request bodies of at least 512 MB for configuration and state uploads. For ingress-nginx,
  set `nginx.ingress.kubernetes.io/proxy-body-size: 512m`.
- Do not buffer streaming responses, and allow long-lived connections for live logs (SSE) and
  job status (WebSocket at `/api/subscriptions`). For ingress-nginx, set
  `proxy-buffering: "off"` and `proxy-read-timeout`/`proxy-send-timeout: "3600"`.

## Setups

- **Helm:** set `ingress.singleDomain.enabled: true` (chart 4.8.0 or later). The chart renders one ingress or HTTPRoute from the route table.
- **Docker Compose:** in the Terrakube repository, run `docker compose -f docker-compose.yml -f docker-compose.single-domain.yml up -d` from `docker-compose/`.

## Known limitations

- Serving Terrakube under a sub-path of another site, such as `https://example.com/terrakube`, is
  not supported.

## Migrating an existing installation

This guide moves an existing split-domain installation (`terrakube.example.com`,
`terrakube-api.example.com`, `terrakube-registry.example.com`) to one hostname. Read the
overview above first.

### Before you start

- Upgrade to 2.35 or later in split-domain mode, and check that everything still works. The
  `/api/...` paths appear once you set `TerrakubePublicUrl`.
- Keep the old hostnames routed to the same services during the migration and after it.
  These things still point at them:
  - run output and state links that are already stored in the database;
  - module sources in your Terraform code, such as `terrakube-registry.example.com/org/module/aws`;
  - webhooks registered with your VCS providers;
  - CLI credentials stored by `terraform login`.
- The discovery document on every hostname now advertises the registry under
  `/api/private-registry/`. If the old API hostname stays routed to the API only, module sources on
  that hostname return "Module not found". Route `/api/private-registry/` to the registry on every
  hostname that serves the API, or keep module sources on the registry hostname.

### Steps

1. **Pick the shared hostname** and issue a certificate for it.
2. **Keep your issuers.** If dynamic credentials are in use, the cloud trust policies are bound to
   the token issuer. Either keep `TerrakubeHostname` set or set `DynamicCredentialHostname` to the
   current issuer host. The issuer then stays the same.

   While both `TerrakubeHostname` and `TerrakubePublicUrl` are set, discovery advertises the
   `/api/...` paths, but the state, upload, log and webhook URLs that the API generates keep
   pointing at `https://<TerrakubeHostname>` without the `/api` prefix. The API logs a warning at
   startup in this state. That keeps clients that only know the old API hostname working. Remove
   `TerrakubeHostname` (and set `DynamicCredentialHostname` if you need the old issuer) once
   clients have moved.
3. **Set `TerrakubePublicUrl`** on the API, the registry and the executor, and
   `REACT_APP_TERRAKUBE_PUBLIC_URL` plus the UI values from the overview in `env-config.js`.
   Remove or update the explicit UI, Dex and registry URLs that should now come from the public URL.
4. **Move Dex** to `<public>/dex`: update its `issuer` and add the new redirect URIs. Terrakube
   accepts one Dex issuer, so changing it signs everyone out once: browser sessions and
   `terraform login` credentials issued by the old issuer are refused on every hostname. Personal
   access tokens and team tokens keep working.
5. **Route the shared host** as described in the overview. Keep the old per-component hosts routed
   as well.
6. **Restart** the components and sign in on the new hostname.

### What users redo

- Sign in again in the browser.
- Run `terraform login <new-host>` once, because credentials from the old Dex issuer are refused. Update `cloud { hostname = "<new-host>" }` blocks when
  convenient. The old hostname keeps working while it stays routed.
- Change module sources to `<new-host>/<org>/<module>/<provider>` over time.

### Deprecated paths

The legacy root paths still work in 2.35. Use the canonical paths for new integrations:

| Legacy path | Canonical path |
| --- | --- |
| `/remote/tfe/v2/...` | `/api/remote/tfe/v2/...` |
| `/tfstate/v1/...`, `/tfoutput/v1/...` | `/api/tfstate/v1/...`, `/api/tfoutput/v1/...` |
| `/graphql/api/v1`, `/subscriptions` | `/api/graphql/api/v1`, `/api/subscriptions` |
| `/oauth/...` | `/api/oauth/...` |
| `/access-token/v1/...`, `/pat/v1` | `/api/access-token/v1/...`, `/api/pat/v1` |
| `/webhook/...`, `/callback/...` | `/api/webhook/...`, `/api/callback/...` |
| `/terraform/modules/v1/...` (registry host) | `/api/private-registry/terraform/modules/v1/...` |
| any other API root path `/<path>` | `/api/<path>` |

`/api/v1/...` (JSON:API) is unchanged.

`/webhook/...` and `/callback/...` stay served on the shared host as well. VCS providers have those
URLs registered, so they will not be removed in a later release.
