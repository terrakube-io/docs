# Team Access

A workspace's **Team Access** tab grants teams access scoped to that single workspace, on top of whatever they already have from [organization](../organizations/team-management.md) or [project](../projects/team-access.md) level permissions. Access is always additive — a role granted here can't take away permissions a team already has at the organization or project level.

{% hint style="info" %}
Managing workspace team access requires the **Manage Workspace** permission on that workspace (from any level of the hierarchy).
{% endhint %}

### Roles

| Role      | Description                                            |
| ---------- | ---------------------------------------------------------- |
| **Admin**  | Full control over this workspace                          |
| **Write**  | Can plan, apply runs, and manage this workspace's settings |
| **Plan** | Can queue runs; does not grant approve permission; enable the workspace approval rule to restrict ungated applies. |
| **Read**   | Read-only access to this workspace and its runs            |
| **Custom** | Fine-grained permissions, picked individually               |

### Custom permissions

When **Custom** is selected:

| Permission        | Description                                     |
| ------------------- | ---------------------------------------------------- |
| Plan Runs            | Queue plan runs on this workspace                    |
| Apply Runs             | Approve/apply runs on this workspace                 |
| Destroy Runs | Start a destroy without an approval step, or approve a destroy run. |
| Manage Workspace      | Administrate this workspace's settings                |
| Manage State           | View the terraform/tofu state for this workspace from the UI |

### Adding a team

Open the workspace, go to **Settings > Team Access**, select a team and a role, and click **Add**.

<figure><img src="../../.gitbook/assets/workspace-team-access.png" alt=""><figcaption><p>Team Access tab, with one team granted the Write role</p></figcaption></figure>

{% hint style="info" %}
This tab can also be managed via the `terrakube_workspace_access` resource in the [Terrakube Terraform provider](https://registry.terraform.io/providers/AzBuilder/terrakube), or the [Workspace Access](../terrakube-cli/commands/workspace-access/) CLI commands.
{% endhint %}

### Run permissions in 2.34.0

Admin and Write include **Destroy runs**; Plan and Read do not. For Custom access, `destroyJob` defaults to **true**, including existing entries on upgrade. Review it when separating planning, deployment and destruction duties. A destroy waiting for approval can be queued by a planner, but the approver also needs destroy permission.

The Plan role does not grant approval permission. To prevent planners from starting an apply through a template without an approval step, enable `requireApproveToApply` on the workspace. Permissions are additive: a grant at another level can still allow the operation. See [Run Restrictions](run-restrictions.md) for the full rules, including ordinary applies that delete resources.
