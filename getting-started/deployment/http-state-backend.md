# Executor HTTP State Backend

Added in Terrakube 2.34.0, the HTTP state backend lets the executor read and write current Terraform state through the Terrakube API. The API persists it in the configured storage backend. The Helm chart (4.7.16) **enables it by default**. Terrakube itself defaults `TerraformStateHttpBackend` to `false`, so deployments that don't use the chart must turn it on.

Use it when you want Terraform state operations to use workspace-scoped executor credentials through the API. Plans, logs, artifacts and binary caches still use the executor's configured storage implementation, so enabling it does not remove all executor storage credentials or access requirements.

### Enable it

Upgrade the API and executors to 2.34.0 before enabling this feature. With the Helm chart it is already on (`storage.httpBackend: true`; `executor.properties.httpBackend` overrides it for the executor). Set either to `false` to keep state on the configured storage only. Without the chart, on each executor, including standalone and ephemeral agents that should use it, set:

```yaml
executor:
  env:
    - name: TerraformStateHttpBackend
      value: "true"
```

Retain the existing storage configuration. The HTTP implementation wraps it for state operations and delegates the other storage operations. Configure equivalent environment variables in your standalone/ephemeral executor deployment as needed.

The executor generates `terrakube_override.tf` with an HTTP backend pointing at:

```text
/tfstate/v1/organization/{organizationId}/workspace/{workspaceId}/backend/terraform.tfstate
```

It passes authentication through `TF_HTTP_USERNAME` and `TF_HTTP_PASSWORD`, keeping credentials out of the generated backend configuration. Users continue to use their existing Terrakube workflow; they do not replace their local `cloud` or `remote` block with this executor endpoint.

### Access and limits

The token the executor uses lasts as long as the executor's maximum job duration (`ExecutorJobMaxDurationMinutes`, 360 minutes by default), so long applies and destroys can still write state at the end. Terrakube revokes it as soon as the step finishes. To set the lifetime separately, use `TerraformStateHttpTokenTtlMinutes` on the executor.

The endpoint accepts scoped `WORKSPACE_EXECUTOR` tokens matching the organization and workspace. It is not a general-purpose HTTP backend for user PATs. Plan steps can read state but cannot write or delete it. Apply, destroy and custom-script flow steps can write/delete state with the required executor token.

| API behavior | Result |
| --- | --- |
| GET with no current state | `204 No Content` |
| POST/PUT with an empty body or mismatched supplied `Content-MD5` | `400 Bad Request` |
| Upload exceeding `TerraformStateMaxSizeBytes` | `413 Payload Too Large` |
| Missing/invalid credentials or insufficient scope | `401` or `403` |

`TerraformStateMaxSizeBytes` is an **API** environment variable with a default of `104857600` bytes (100 MiB). Change it if your state requires a different upload limit. Writes and deletes log organization, workspace, job and step identifiers for audit context.

This endpoint does not implement HTTP backend lock/unlock operations. Terrakube continues to serialize runs per workspace; do not treat it as a separately managed state service for external writers.

See [Terraform State](../../user-guide/workspaces/terraform-state.md) for viewing versions, and the [API](../../user-guide/reference/configuration/api.md) and [executor](../../user-guide/reference/configuration/executor.md) configuration references for exact environment variable names.
