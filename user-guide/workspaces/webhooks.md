# Webhooks

{% hint style="info" %}
**Manage Workspaces** permission is required to perform this action, please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Webhooks let a Version Control workflow workspace react automatically to activity in its git repository: kicking off a plan/apply job on a push, or posting plan and apply results as comments on a pull request. This page covers the **Webhook** tab inside a workspace's settings. For connecting Terrakube to your VCS account in the first place, see [VCS Providers](../vcs-providers/ "mention").

### Enabling the webhook

Open a workspace, go to **Settings > Webhook**, and turn on **Enable VCS Webhook?**. Once enabled, Terrakube registers a webhook with your VCS provider and shows its remote ID.

On GitHub and GitLab repositories you can optionally click **Migrate to Shared Webhook** to consolidate the webhooks for every workspace pointed at the same repository into a single shared webhook, instead of one per workspace. This is experimental; **Revert** switches a workspace back to its own per-workspace webhook.

### Webhook events

Each row in the events table matches a specific kind of repository activity to a template:

| Field         | Description                                                                                                                                                     |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Priority      | When more than one event row of the same type could match a payload, Terrakube evaluates them in ascending priority order and uses the first one whose branch and file filters match. |
| Event         | `Push`, `Pull Request`, or `Release`. See provider support below — not every VCS supports every event type.                                                    |
| Branch/release | Comma-separated list of branch or release names, matched as a regex against the branch/release in the payload.                                                |
| Path Type     | `Pattern` for simple wildcards like `terraform/*` or `modules/**`, or `Regex` for full regular-expression matching against changed files. Branch/release matching always uses regex regardless of this setting. |
| File          | The pattern or regex list (depending on Path Type) that changed files must match for the job to run. Useful for monorepos so unrelated changes don't trigger a run. |
| Template      | The [template](../organizations/templates/ "mention") to run when this event matches.                                                                          |

### Posting plan and apply results on pull requests

For `Pull Request` events, two independent switches control an Atlantis/TFE-style PR comment workflow:

| Toggle                         | Effect                                                                                                                                                                                 |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Post Plan on PR**              | Terrakube posts the plan output as a comment on the pull request, and accepts `terrakube plan` PR comments to re-run the plan.                                                        |
| **Allow Apply via PR Comment**   | Accepts `terrakube apply` PR comments to run the workspace's default template with `auto-apply`. Only selectable once **Post Plan on PR** is enabled.                                 |

If **Allow Apply via PR Comment** is off, the plan comment shows a notice instead of the apply instructions, and a `terrakube apply` comment gets a reply explaining apply is disabled rather than being silently ignored:

{% hint style="info" %}
Ask a workspace admin to enable **Allow Apply via PR Comment** in the Webhook settings, or apply the plan from the Terrakube UI.
{% endhint %}

**What a plan comment looks like:**

* A header with the workspace name, status, and a link back to the run in the Terrakube UI.
* A status icon (✅ / ❌ / ⚠️) and a one-line summary (e.g. `Plan: 2 to add, 0 to change, 0 to destroy.` or `No changes.`).
* The full plan output in a collapsible, diff-highlighted block.
* A footer inviting `terrakube apply` (only when enabled) and `terrakube plan` to re-run.

**Replans update the existing comment.** Pushing new commits to the same PR edits the existing plan comment in place instead of posting a new one every time — if the edit fails (for example the original comment was deleted), Terrakube falls back to posting a new comment. Apply results always post as their own separate comment, so the apply history is never overwritten.

**Command acknowledgment.** As soon as Terrakube recognizes a `terrakube plan` or `terrakube apply` comment, it reacts to that comment with 👀 so you know it was seen. Once the triggered job finishes, the reaction updates to ✅ (success) or ❌ (failure). Bitbucket has no comment-reaction API, so acknowledgment is a no-op there — Bitbucket users still get the plan/apply result comment itself.

<figure><img src="https://github.com/user-attachments/assets/e40adb29-82db-442f-aa5e-5cc7f08cffff" alt=""><figcaption><p>GitHub pull request with a Terrakube plan comment</p></figcaption></figure>

<figure><img src="https://github.com/user-attachments/assets/02aa0b0b-293c-4068-b7f3-4e9db1f267ac" alt=""><figcaption><p>GitLab merge request with a Terrakube plan comment</p></figcaption></figure>

### Provider support

| Provider                                              | Push | Pull Request | Release | Post Plan on PR / Apply via PR Comment |
| ------------------------------------------------------ | :--: | :----------: | :-----: | :-------------------------------------: |
| [GitHub](../vcs-providers/github.com.md)                | ✅   | ✅            | ✅      | ✅ (with 👀→✅/❌ reactions)               |
| [GitHub Enterprise](../vcs-providers/github-enterprise.md) | ✅   | ✅            | ✅      | ✅ (with 👀→✅/❌ reactions)               |
| [GitLab](../vcs-providers/gitlab.com.md) / [GitLab EE/CE](../vcs-providers/gitlab-ee-and-ce.md) | ✅   | ✅            | ✅      | ✅ (with 👀→✅/❌ award emoji)             |
| [Bitbucket](../vcs-providers/bitbucket.com.md)          | ✅   | ✅            | ✅      | ✅ (no reactions — no comment-reaction API) |
| [Azure DevOps](../vcs-providers/azure-devops.md)        | ✅   | ✅            | –       | Not yet supported                        |
| [SSH](../vcs-providers/ssh.md)                          | –    | –             | –       | –                                         |

Azure DevOps triggers push and pull-request jobs via service hooks (or outbound polling on private networks — see [Azure DevOps](../vcs-providers/azure-devops.md#webhooks)), but the PR comment plan/apply workflow above is currently only available for GitHub, GitLab, and Bitbucket. SSH-based repositories have no webhook support at all; runs must be triggered manually, via the API, or on a [schedule](workspace-scheduler.md).
