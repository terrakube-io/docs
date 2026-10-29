# Token Security

Terrakube uses two secrets that you must set yourself:

| Key | Used for |
| --- | --- |
| `security.patSecret` | Signing personal access and team tokens. |
| `security.internalSecret` | Authenticating calls between the API, executor and registry. |

```yaml
security:
  patSecret: "<32 characters>"
  internalSecret: "<a different 32 characters>"
```

Each must be 32 characters long and base64-compatible. Generate them with:

```bash
openssl rand -hex 16
```

{% hint style="danger" %}
The values shipped in the Helm chart, and in the examples in this documentation, are public. Always replace them in a real deployment, and use different values for each environment.
{% endhint %}

To keep the secrets out of your values file, create the Kubernetes secrets yourself and set `security.existingSecret: true`, then see the chart's `values.yaml` for the secret names it expects.
