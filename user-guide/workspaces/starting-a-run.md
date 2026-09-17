# Starting a Run

Open a workspace and click **New Run** to start a job without waiting on a git push or the API. This is the native equivalent of the [CLI-driven](cli-driven-workflow.md "mention") and [API-driven](api-driven-workflow.md "mention") run-creation paths, and works for any workspace regardless of workflow type.

| Field | Description |
| --- | --- |
| Choose job type | The [template](../organizations/templates/ "mention") to run, e.g. `Plan`, `Plan and Apply`, or a `Destroy` variant (shown in red). |
| Branch Name | The git branch to run against. Leave as-is for CLI-driven workspaces — Terrakube manages this value itself in that workflow. |

### Additional planning options

Expand **Additional planning options** to scope the plan the same way `terraform plan -target=... -replace=...` would from the CLI:

| Field | Description |
| --- | --- |
| Target resources | Limits the plan to these resource addresses and their dependencies. |
| Replace resources | Forces replacement of these resource addresses on the next apply. |

Both fields autocomplete against the workspace's actual state resources — start typing to search, or type a full address (e.g. `aws_instance.example` or `module.foo.aws_instance.bar`) and press Enter to add it as free text.

{% hint style="info" %}
Target and replace addresses only apply to the run you're creating — they aren't saved on the workspace and don't affect future runs, including ones triggered by a webhook or the scheduler.
{% endhint %}
