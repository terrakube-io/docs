# Upgrading

Terrakube is upgraded by deploying a newer version of all components together. Database changes are applied automatically when the API starts.

### Before you upgrade

1. **Back up the database.** Migrations only run forward. To roll back after a migration has run, restore the backup you took before upgrading.
2. **Read the notes below** for every version between yours and the target, and the [Updates](../updates.md) page.
3. **Upgrade together.** Deploy the API, executor, registry and UI at the same version. Short skew during a rolling upgrade is supported (see the notes), but do not run mixed versions for long.
4. **Use a maintenance window for large changes** and let running jobs finish first. A run that is executing when its executor restarts can fail.

```bash
helm repo update
helm upgrade terrakube terrakube-repo/terrakube -n terrakube --values terrakube.yaml --version <chart version>
```

`helm repo update` first: a stale local index shows an older chart than the latest.

### Chart and Terrakube versions

Each chart release has a default Terrakube version (its `appVersion`). You can run a newer Terrakube by setting `api.version`, `executor.version`, `registry.version` and `ui.version`, but some releases need a minimum chart because they add chart values or templates.

| Terrakube | Latest chart that ships it | Minimum chart and why |
| --- | --- | --- |
| 2.34.0 | not yet released | **4.7.12**, for `executor.emptyDirSecret`. 4.7.11 for `api.ephemeralExecution.*` job controls |
| 2.33.x | 4.7.12 (2.33.1) | **4.7.7**, for safe multi-replica executors |
| 2.32.x | 4.7.8 | |
| 2.31.x | 4.7.3 | |
| 2.30.x | 4.6.2 | |
| 2.29.0 | 4.5.6 | |
| 2.28.x | 4.5.1 | |

Chart 4.7.12 is the latest at the time of writing. Its default Terrakube version is 2.33.1, so set the component versions explicitly to get 2.33.3 or a 2.34 pre-release.

### Terrakube 2.34.0

These come from the changes in the 2.34 release line. Check each against your deployment.

**Action required**

* **Executor secret volume.** The executor now reads its internal secret from a file and removes it after loading. Set `executor.emptyDirSecret: true` (chart 4.7.12+) for every executor deployment, including [standalone agents](deployment/self-hosted-agents.md). See [Helm Chart](deployment/helm-chart.md#upgrading).
* **Ephemeral executors.** Ephemeral jobs now get their secret mounted as files by default (`ExecutorEphemeralEmptyDirSecret=true`). The executor image they use must be 2.34.0 or later, or set the variable to `false`. See [Ephemeral Agents](deployment/ephemeral-agents.md#executor-secrets-in-ephemeral-jobs).

**Behaviour changes**

* Step output is **no longer written to the executor's own log** (`kubectl logs`) by default. Set `ExecutorLogsEchoToServiceLog=true` if you read output there. See [Executor Log Output](deployment/executor-log-output.md).
* Creating a run now requires permission to plan in the workspace. Users and teams with read-only access can no longer create runs.
* A run with unresolved policy violations cannot be approved, and changing a module or provider version requires the manage module or provider permission.
* Runs waiting for approval are **not** closed by the inactive-run limits unless you set `InactiveJobsWaitingApprovalTimeoutHours`. Other open runs are closed as failed after `InactiveJobsMaxAgeHours` (default 6). See [Executor Log Output](deployment/executor-log-output.md#run-limits).
* Features that are **off until you enable them**: [`terraform login` through Terrakube](../user-guide/workspaces/cli-driven-workflow.md#terraform-login), presigned module downloads in the registry, and plan artifacts.

**Changes since 2.34.0-beta.10**

These changes are included in 2.34.0-rc.1 and apply when upgrading from beta.10 or earlier:

* **Token provisioning:** PATs/team tokens/federated tokens cannot mint more tokens by default. Provisioning tools that do this need a Dex user session or a deliberate `TokenAllowPatIssuance=true` compatibility choice for PATs. Children cannot outlive the parent PAT. Unknown or revoked Terrakube token IDs are rejected. Review [Token Security](deployment/token-security.md).
* **Destroy permission:** Admin/Write retain destroy access; Plan/Read cannot destroy. Custom team, workspace and project entries get `destroyJob=true` to preserve previous access. Review and remove that grant where appropriate. Approving a destroy also requires it.
* **CLI runs:** auto-approve requires approve permission. Supplying `-var`, `-var-file` or `TF_VAR_*` run inputs requires workspace-management permission; the values are now used rather than silently ignored. Project-level plan and approve grants are honored by the Terraform-compatible API.
* **Sensitive values and tenant checks:** workspace variables and collection items cannot change from sensitive to non-sensitive. Cross-organization resource relationships and template lookups are rejected. Check integrations that depended on those operations.
* **Schedule clients:** stop sending the deprecated `tcl` field; use `templateReference` instead.
* **Opt-in controls:** `restrictApplyToConfiguredBranch` and `requireApproveToApply` both default to false. Branch restrictions conflict with API-created exact-commit apply runs, including the documented GitHub Action deployment flow. Choose controls using [Recommended Settings](recommended-settings.md).
* **New environment controls:** `TokenAllowPatIssuance=false`, `TokenMaxDays=0` (no configured maximum), and `DexAudienceValidation=false` are the shipped defaults. The [HTTP state backend](deployment/http-state-backend.md) is also off by default.
* **Cloud credentials:** tighter AWS/GCP setup examples do not update existing cloud policies. Review and apply the trust changes in your cloud account.

The API migration adds workspace restriction columns, destroy flags on all three access levels, and CLI-variable placement metadata on jobs. Finish upgrading the API and executors before relying on branch commit verification, typed CLI inputs or HTTP state access. An older executor does not enforce the new commit-membership check. Both workspace controls remain off until enabled.

**Rolling upgrades**

* **Upgrade the API before the executors.** Structured-output recovery adds step columns in the API's migration. A new executor against an older API falls back to the old behaviour, and an older executor against a new API keeps working, but results are only recoverable once both are on 2.34. In general an executor ahead of the API, or an API ahead of the executor, works for runs that do not use new features. Finish the rollout before clients send [run-specific variables](../user-guide/workspaces/api-driven-workflow.md#variables-for-one-run): an older API pod would run the job without them. `GET /execution/v1/capabilities` reports `jobVariables: true` on a new API.
* Run triggers, policy governance and registry version status add tables or columns. They are created by the automatic migration and are empty until used.

**Cloud storage**

* S3-compatible stores that need path-style addressing off, a signing region, or no chunked encoding or checksums can now set them. See [Amazon Cloud Storage](deployment/storage-backend/amazon-cloud-storage.md).

### Terrakube 2.33

* **Chart 4.7.7 or later** for multiple executor replicas. Each executor pod accepts one job at a time and reports not-ready while it runs one, so Kubernetes stops sending it work. See [Self-Hosted Agents](deployment/self-hosted-agents.md#scaling-to-multiple-replicas).
* **Redis is required for the scheduler.** The scheduler takes a Redis lock before dispatching a run and does not dispatch if Redis is unreachable, so a Redis outage pauses new runs instead of risking duplicates.
* Terraform and OpenTofu binaries are cached in your storage backend rather than downloaded per run.

### Terrakube 2.29

* Terrakube moved to **Java 25**. If you build custom images, use a Java 25 runtime.

### Rolling back

Restore the database backup, then deploy the previous chart and component versions. Running an older API against a database that a newer API has migrated is not supported.
