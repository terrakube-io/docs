# Troubleshooting

Find the symptom, check the likely causes in order, and follow the link for the fix. Where a command is shown, replace `terrakube` with your namespace and release names.

### A run stays `pending`

A run is admitted only when its workspace is free and an executor is ready. Check, in order:

1. **An earlier run in the same workspace is still active** (`pending`, `queue`, `running`, `approved` or `waitingApproval`). Runs in a workspace go one at a time. Open the **Runs** tab and finish, approve, discard or cancel the run in front. A run **waiting for approval** holds the queue until someone acts on it.
2. **No executor is ready.** Each executor pod runs one job at a time, so a busy pool leaves later runs pending. Add replicas, see [Self-Hosted Agents](../../getting-started/deployment/self-hosted-agents.md), or use [ephemeral executors](../../getting-started/deployment/ephemeral-agents.md). Pending is retried, it does not fail.
3. **Redis is unreachable.** The scheduler will not dispatch without its Redis lock, so an outage pauses new runs. Check the Redis connection settings in [External Redis](../../getting-started/deployment/external-redis.md) and the API logs.
4. **The workspace is locked.** A locked workspace does not start new runs. Unlock it under **Settings → Locking**.

### A run was "Closed by Terrakube"

Runs open too long are closed: `pending`, `queue`, `running` and `approved` runs fail after `InactiveJobsMaxAgeHours` (default 6), and runs waiting for approval are cancelled only if you set `InactiveJobsWaitingApprovalTimeoutHours`. The reason is shown at the top of the run. See [Run limits](../../getting-started/deployment/executor-log-output.md#run-limits).

### A step failed with `x509: certificate signed by unknown authority`

The executor reached a server with a certificate it does not trust, for example `releases.hashicorp.com`, `registry.terraform.io`, `registry.opentofu.org` or your Git host. This is almost always a corporate proxy that inspects TLS, or a private CA.

* Add your CA certificate: [Custom CA Certs](../../getting-started/deployment/custom-ca-certs.md).
* If traffic must go through a proxy: [Proxy Configuration](../../getting-started/deployment/proxy-configuration.md).
* Re-run after the executor pods have restarted with the certificate.

### `terraform init` cannot download a module or provider from the registry

* `401` or `403`: log in with `terraform login <registry host>`, and log in to the API host and the registry host separately if they differ. See [CLI-driven Workflow](../workspaces/cli-driven-workflow.md#terraform-login).
* The registry needs **HTTPS**; Terraform and OpenTofu will not use a plain-HTTP registry.
* The version is missing: it may have been [removed](../private-registry/deprecating-and-removing-versions.md), or it is a new git tag that has not been picked up yet. The version list refreshes every few minutes; see [Private Registry](../private-registry/README.md).
* A module version only appears for tags that are valid semantic versions, such as `v1.2.3`.

### `terraform login` fails or hangs

* After a successful login the browser may show a connection error to `localhost`. That is expected, because Terraform closes its temporary listener once it has the token.
* The sign-in page rejects the redirect: add the redirect URIs to your Dex static client, including `<API URL>/oauth/callback` when you use the Terrakube login broker. See [User Authentication (Dex)](../../getting-started/deployment/user-authentication-dex.md#terraform-login).
* The API fails to start with `TerraformLoginApiUrl` errors: the login broker needs a public **HTTPS** API URL. See [CLI-driven Workflow](../workspaces/cli-driven-workflow.md#login-through-the-terrakube-oauth-broker).

### I cannot sign in to the UI

* **Redirect loop or "invalid issuer":** `dex.config.issuer` must be exactly the public API URL plus `/dex`. See [Ingress Configuration](../../getting-started/deployment/ingress-configuration.md).
* **Signed in but can do nothing:** your identity provider must send a **groups** claim, and your group must match a [team](../organizations/team-management.md) (or the administrator group, `security.adminGroup`, for creating organizations).
* **New users get no access:** group names are case sensitive and must match the team name exactly.

### A webhook does not start a run

1. Open the workspace **Settings → Webhook** and confirm it is enabled and the event matches: events are checked top to bottom and the first match wins, so an earlier event can shadow a later one.
2. Check the branch pattern (a regular expression) and the changed-files filter. **Test patterns** lets you try a sample branch or path.
3. In your Git provider, look at the webhook's recent deliveries for errors. If the hook was deleted there, Terrakube recreates a shared webhook; for a per-workspace hook disable and enable it again.
4. If you changed the workspace's repository or VCS provider, the webhook was removed on purpose; enable it again. See [Webhooks](../workspaces/webhooks.md).

### Logs are missing or the Console says output is incomplete

* While a step runs, logs stream through Redis. If they stop, check that Redis is healthy.
* A warning that the step's output is **incomplete** means the executor ended before it saved the final output; the Console shows what was archived so far. The run itself is unaffected. See [Job Output](../workspaces/job-output.md).
* The warning needs your proxy or ingress to expose the `X-Terrakube-Output-Incomplete` response header to the browser (`Access-Control-Expose-Headers`).
* Since 2.34, output is not copied to the executor's own log. To read it with `kubectl logs`, set `ExecutorLogsEchoToServiceLog=true`.

### A notification was not delivered

Open the workspace **Settings → Notifications** and read **Recent deliveries**: it shows the error and has a **Retry** button. Destination URLs that point at private or internal networks are rejected, and a Slack, Teams or webhook endpoint that returns an error is retried with backoff. See [Notifications](../workspaces/notifications.md).

### A policy check fails or blocks the run

Open the run and its **Policy checks** panel for the policy set, rule and resource. Fix the plan, or have the override team override a soft-mandatory violation, or add an [exemption](../policy-enforcement-opa.md#exemptions). If the policy step itself errors, check that the policy repository, branch, folder and VCS connection of the policy set are reachable from the executor.

### Ephemeral executor jobs fail immediately

Ephemeral jobs are deleted shortly after they finish (30 seconds by default), which can remove the evidence. Raise `ExecutorEphemeralTtlSecondsAfterFinished` while debugging, then `kubectl describe pod` and `kubectl logs` on the job's pod. The usual causes are an image that cannot be pulled, a missing secret, or running a pre-2.34 executor image with a 2.34 API. See [Ephemeral Agents](../../getting-started/deployment/ephemeral-agents.md) and [Upgrading](../../getting-started/upgrading.md).

### The executor will not start after upgrading to 2.34

Set `executor.emptyDirSecret: true` (chart 4.7.12 or later). See [Upgrading](../../getting-started/upgrading.md#terrakube-2340).

### S3-compatible storage returns `403 SecondLevelDomainForbidden`

The store only accepts virtual-hosted-style requests. Set `AwsPathStyleAccess=false` on the API, executor and registry. See [Amazon Cloud Storage](../../getting-started/deployment/storage-backend/amazon-cloud-storage.md).

{% hint style="info" %}
Still stuck? Collect the run link, the step output, and the API and executor logs, and open an issue at [terrakube-io/terrakube](https://github.com/terrakube-io/terrakube/issues).
{% endhint %}
