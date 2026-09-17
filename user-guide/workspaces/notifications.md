# Notifications

{% hint style="info" %}
**Manage Workspaces** permission is required to create or edit notification configurations, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Terrakube can post to Slack, Microsoft Teams, or any generic webhook endpoint when a job's status changes — a run finishing, failing, or landing in **Waiting for Approval**. Configurations can be created at two scopes:

* **Organization** (**Organization Settings > Notifications**) — applies to every workspace in the organization.
* **Workspace** (workspace **Settings > Notifications**) — applies to that workspace only.

Both scopes are additive: if an organization-wide configuration and a workspace-specific one both match a job's new status, both fire. There's no override or suppression between them — a workspace can't turn off an inherited organization notification, only add its own alongside it.

### Creating a notification configuration

{% hint style="info" %}
_Screenshot pending: notification configuration list (Settings > Notifications), showing scope and channel._
{% endhint %}

From **Settings > Notifications**, click **New Notification** and choose a channel:

| Field | Description |
| --- | --- |
| Channel | **Slack**, **Microsoft Teams**, or **Generic Webhook** — see [Channels](notifications.md#channels) below. |
| Name | A label for this configuration, e.g. `Prod Alerts`. Shown on delivered messages so additive org + workspace notifications for the same run are easy to tell apart. |
| Description | Optional free text — what this configuration is for. |
| Destination URL | The channel's incoming webhook URL (Slack/Teams) or any HTTPS endpoint (Generic Webhook). Validated against private/loopback/link-local/reserved address ranges before every send, including test sends. |
| Signing Secret | Generic Webhook only. If set, every delivery includes an `X-Terrakube-Signature` header — an HMAC-SHA256 hex digest of the raw JSON body, keyed with this secret — so your endpoint can verify the payload came from Terrakube. |
| Active | Turns delivery on or off without deleting the configuration. |
| Message style | Slack/Teams only. **Detailed** sends the full card (status, workspace, job, commit, failure reason, run link). **Simple** sends a single-line message. |
| Triggers | Which job statuses fire this configuration — grouped as *Needs Attention* (Waiting for Approval), *Completed* (Completed, Completed with No Changes), *Errored* (Failed, Rejected, Cancelled), *In Progress* (Pending, Approved, Queued, Running), and *Other*. |
| Templates | Optional. Restrict this configuration to specific [templates](../organizations/templates/ "mention"), e.g. only `Plan and Apply`, not `Plan`. Leave empty to match every template. |

Click **Send Test** at any point — including before saving — to deliver a real payload to the destination URL and confirm it's wired up correctly.

### Channels

| Channel | Payload | Notes |
| --- | --- | --- |
| **Slack** | Slack Block Kit message with a status-colored side bar | Create an [Incoming Webhook](https://api.slack.com/messaging/webhooks) in Slack and paste its URL as the destination. |
| **Microsoft Teams** | Adaptive Card | Create an [Incoming Webhook connector](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook) on the target channel and paste its URL as the destination. |
| **Generic Webhook** | Raw JSON POST containing workspace, job, status, commit ID, run URL, and failure reason (when applicable) | Any HTTPS endpoint. Add a **Signing Secret** to verify deliveries came from Terrakube. |

### Delivery history and retries

{% hint style="info" %}
_Screenshot pending: delivery history table on a workspace's Settings > Notifications page._
{% endhint %}

A workspace's **Settings > Notifications** page also lists recent delivery attempts for that workspace, across every configuration (its own and inherited from the organization) — status, channel, and timestamp.

Deliveries are queued through a transactional outbox rather than sent inline with the job status update, so a crash between the two can't lose a notification; a background poller sweeps up anything stuck and retries it automatically. Terrakube distinguishes:

* **Retryable failures** — timeouts, `429`, and `5xx` responses (honoring `Retry-After` when the channel sends one) — retried automatically.
* **Terminal failures** — other `4xx` responses, e.g. an invalid or revoked webhook URL — not retried automatically. Use **Retry** on the delivery history row once the underlying problem (URL, permissions) is fixed.

{% hint style="warning" %}
Destination URLs are validated against private, loopback, link-local, and other reserved address ranges before every send, and Terrakube does not follow redirects from a validated destination. Self-hosted deployments that need to notify an internal endpoint can relax this via server-side configuration — see the API's `application.properties`.
{% endhint %}
