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

### Token creation and lifetime

From 2.34.0, PAT and team-token creation requires a user signed in through Dex by default. Existing PATs, team tokens and federated workload tokens cannot normally mint more tokens. This prevents automation from extending access by creating new credentials.

| API variable | Default | Behavior |
| --- | --- | --- |
| `TokenAllowPatIssuance` | `false` | When enabled, an active PAT may create PATs or team tokens. A child must expire no later than its parent; a PAT without expiry cannot mint children. Team-token creation still requires group membership. Team and federated tokens cannot mint tokens. |
| `TokenMaxDays` | `0` | Maximum lifetime of PATs/team tokens created through `/pat/v1` and `/access-token/v1/teams`, in days; `0` means no configured maximum. Negative lifetime inputs are rejected. Existing tokens are not shortened. |

Prefer keeping PAT-based issuance disabled and using [federated identity](../../user-guide/organizations/federated-identity.md) for automation. If a provisioning integration currently uses a PAT to create tokens, update its authentication or deliberately enable the bounded compatibility option before upgrading it.

The [Terraform login broker](../../user-guide/workspaces/cli-driven-workflow.md#terraform-login) has its own `TerraformLoginDefaultDays` and `TerraformLoginMaxDays` controls. Configure those consistently with your lifetime policy: `TokenMaxDays` does not cap tokens issued by the broker.

### Dex audience validation

2.34.0 adds optional audience validation for Dex tokens. Set `DexAudienceValidation=true` on both the API and registry after checking that tokens include their expected client IDs: `DexClientId` for the API and `AppClientId` for the registry. Add `DexExtraAudiences` as a comma-separated list only when additional client IDs should be trusted.

The default is **false** to preserve existing sign-in configurations during upgrade. Enabling it rejects Dex tokens whose `aud` does not include an accepted client ID. Test UI sign-in, CLI authentication and registry access with your Dex setup before enabling it everywhere. Federated identities use their separately configured issuer, audience and claim conditions.

### Revocation and federated identity

Terrakube-issued PATs and team tokens must have a known, active token ID in Terrakube's database; missing, unknown or revoked IDs are rejected. Back up the database alongside signing secrets when moving an instance: possessing the signing key alone does not make an unrecorded token valid.

Deleting a team token requires membership in that token's group or instance-owner authorization. Federated workloads receive the identity mapped by Terrakube's federated credential, rather than extra privileges from an external token's groups or email claims. See [API Tokens](../../user-guide/organizations/api-tokens.md) and [Recommended Settings](../recommended-settings.md).
