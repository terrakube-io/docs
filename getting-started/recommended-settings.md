# Recommended Settings

Use this as a starting point for a production deployment. It explains recommended choices; it does not describe settings that Terrakube automatically enables. The run restrictions and token controls below were added between **2.34.0-beta.10 and 2.34.0-rc.1** and are part of the 2.34.0 release line. See [Upgrading](upgrading.md) for changes that take effect on upgrade.

### Where to apply the recommendations

Follow [Production Setup](production-setup.md) for the complete setup order. Apply these recommendations at the corresponding stage:

| Stage | Configure here | Settings covered below |
| --- | --- | --- |
| Deploy the services | Helm `api.env` / `registry.env`, or the service's container environment | Token creation/lifetimes and Dex audience validation |
| Create teams and access entries | Organization **Settings > Teams**, project **Teams**, workspace **Settings > Team Access** | Plan, approve and destroy permissions |
| Prepare templates | Organization templates | Named approval team and `requireSavedPlan` |
| Configure each workspace | Workspace **Settings > General** | `restrictApplyToConfiguredBranch` and `requireApproveToApply` |
| Configure cloud access | Workspace variables plus cloud trust policy | Dynamic provider credentials and exact workspace trust |

Service environment variables belong in the deployment. Workspace restrictions belong in workspace settings. The [Configuration Reference](../user-guide/reference/configuration/README.md) lists environment names and defaults; the guides below explain how to choose and apply them.

### Choose how production changes are deployed

| Workflow | Recommended setup | Why and trade-off |
| --- | --- | --- |
| Git branch drives deployment | Enable `restrictApplyToConfiguredBranch`; protect the configured branch in your VCS. Use plan-only templates for PRs and an approval template for deployment. | Keeps apply/destroy on reviewed branch content. CLI uploads and API runs pinned with `commitId` cannot apply. |
| CI deploys an exact commit | Keep `restrictApplyToConfiguredBranch` off for that workspace. Protect the CI branch/environment, narrowly scope its federated identity, and use a saved-plan approval template. | Preserves exact-commit workflows such as the GitHub Action deployment example. CI controls which revision may deploy. |
| CLI uploads configuration | Enable `requireApproveToApply` and use CLI templates with approval steps. Grant plan and approve rights separately where review is required. | There is no Git branch for the workspace to enforce. The permission setting prevents planners from choosing an apply template without a gate. |

Enable **Require the approve permission to start an apply with no approval step** (`requireApproveToApply`) on production workspaces in each workflow. It defaults to `false`. This closes the permission gap when a template omits approval; it does not create a human review step. Keep an explicit approval step where review is required.

Set both workspace controls in **Workspace > Settings > General**, as an organization-level workspace manager. Their defaults are off. Read [Run Restrictions](../user-guide/workspaces/run-restrictions.md) before enabling them, especially for webhooks, release events and exact-commit CI.

### Grant only the run permissions each team needs

Give development teams Plan access, deployment teams the required approve permission, and a smaller operational team destroy permission where appropriate. Use Custom roles when the preset Write role grants too much: Write includes destroy.

Review **Destroy runs** (`destroyJob`) after upgrading. Custom team, project and workspace access entries default to `true` to preserve previous access. Removing it at workspace level cannot cancel a grant from another level. The permission covers destroy workflows; use policies to restrict deletions in ordinary apply plans.

Protect template management too: templates determine whether an approval step exists and which team approves it. Always name the approval team; keep the planning identity out of that team when a separate reviewer is required. For plan/apply across separate steps, set `requireSavedPlan: true` so apply fails if the reviewed plan is unavailable. See [Run Restrictions](../user-guide/workspaces/run-restrictions.md) and [Templates](../user-guide/organizations/templates/README.md#require-a-saved-plan).

### Limit token creation and lifetime

| Setting | Shipped default | Recommended choice | Where and why |
| --- | --- | --- | --- |
| `TokenAllowPatIssuance` | `false` | Keep `false` | API. Require a Dex user session to mint PATs/team tokens rather than letting automation mint more credentials. Prefer federated identity for CI. |
| `TokenMaxDays` | `0` (no configured maximum) | Set a lifetime matching your rotation process, for example `30` days | API. Bounds PAT/team-token creation through their token endpoints. Existing tokens are not shortened; the login broker has a separate limit below. |
| `DexAudienceValidation` | `false` | Enable after checking the audiences of your Dex tokens | API and registry. Rejects Dex tokens intended for another client. Configure `DexExtraAudiences` only for additional client IDs you intentionally trust. |

For example, after verifying Dex audiences, these Helm values apply the controls:

```yaml
api:
  env:
    - name: TokenAllowPatIssuance
      value: "false"
    - name: TokenMaxDays
      value: "30"
    - name: DexAudienceValidation
      value: "true"
registry:
  env:
    - name: DexAudienceValidation
      value: "true"
```

Use your actual client IDs when checking audiences: the API uses `DexClientId`, and the registry uses `AppClientId`. See [Token Security](deployment/token-security.md) for compatibility and issuance details. The Terraform login broker has its own [lifetime settings](../user-guide/workspaces/cli-driven-workflow.md#terraform-login); configure those consistently with your token policy too. For a 30-day policy, set `TerraformLoginMaxDays=30` and keep `TerraformLoginDefaultDays` at or below 30 on the API when the broker is enabled. `TokenMaxDays` alone does not cap broker-issued tokens.

Replace the chart's public signing-secret examples with independent secrets for each environment. Prefer [Federated Identity](../user-guide/organizations/federated-identity.md) for automation, with exact repository, branch or environment claim conditions.

### Scope cloud credentials to the workspace

Use [dynamic provider credentials](../user-guide/workspaces/dynamic-provider-credentials/README.md) instead of stored cloud keys. For AWS, match the exact workspace subject in the role trust policy. For GCP, match the exact subject and workspace ID. An organization-wide wildcard intentionally lets more workspaces assume the role; use it only when that is your intended boundary.

The 2.34.0 setup examples were tightened, but upgrading Terrakube does not change existing AWS or GCP trust policies. Review and apply those changes in the cloud configuration.

### Choose state storage deliberately

The [executor HTTP state backend](deployment/http-state-backend.md) is on by default in the Helm chart (`storage.httpBackend: true`). Terrakube itself defaults `TerraformStateHttpBackend` to `false`, so set it yourself if you don't use the chart. It routes state through the API using workspace-scoped executor credentials, while plans and other artifacts still use the configured storage implementation. It does not remove every need for executor storage access.

### Check the result before wider adoption

Try the settings on one representative workspace: plan a PR, deploy through the intended path, and check that a planner cannot approve or start an ungated apply. Check destroy permissions using access configuration and a disposable workspace if execution is needed. Verify token creation, CLI login and registry access after changing token policy, then apply the same choices to other production workspaces.
