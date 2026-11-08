# Docker Compose

Docker Compose runs the whole stack on one machine, for evaluation and demos. For production use the [Helm chart](deployment/helm-chart.md). If you want to change Terrakube itself, use the [Dev Container](development/dev-container.md) instead.

### Requirements

* Docker Engine or Docker Desktop with the Compose plugin
* [mkcert](https://github.com/FiloSottile/mkcert#installation), to create a locally trusted HTTPS certificate

No custom Docker network, DNS server or `/etc/hosts` entries are needed. The stack uses the `*.localhost` names, which resolve to your machine on their own.

### Start Terrakube

```bash
git clone https://github.com/terrakube-io/terrakube.git
cd terrakube/docker-compose

mkcert -install

mkcert -key-file privkey.pem -cert-file fullchain.pem \
  localhost terrakube.localhost terrakube-api.localhost \
  terrakube-registry.localhost terrakube-executor.localhost \
  terrakube-dex.localhost
cp "$(mkcert -CAROOT)/rootCA.pem" rootCA.pem

docker compose up -d
```

Restart your browser once after `mkcert -install`. After a minute or two open <https://terrakube.localhost> and sign in with `admin@example.com` and password `admin`.

| URL | Service |
| --- | --- |
| `https://terrakube.localhost` | UI |
| `https://terrakube-api.localhost` | API |
| `https://terrakube-registry.localhost` | Registry |
| `https://terrakube-dex.localhost` | Dex sign-in |

The certificate, private key and root CA are local files. Do not commit them.

### Choose the version and options

Settings are in `docker-compose/.env`. The ones you will most likely change:

| Variable | Description |
| --- | --- |
| `TK_VERSION` | Terrakube image version for the API, executor, registry and UI. Set it to the release you want to try; the value in the repository can be older than the latest release. |
| `DOMAIN` | Base domain, `localhost` by default. |
| `TERRAFORM_LOGIN_ENABLED` | Set to `true` to enable [`terraform login`](../user-guide/workspaces/cli-driven-workflow.md#terraform-login) through Terrakube. |

Restart with `docker compose up -d --force-recreate` after changing them.

{% hint style="warning" %}
The bundled S3-compatible storage, PostgreSQL, Valkey and LDAP are for local use only, with default passwords.
{% endhint %}

Thanks to [SolomonHD](https://gist.github.com/SolomonHD/b55be40146b7a53b8f26fe244f5be52e) for the original Traefik setup.
