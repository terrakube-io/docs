# Run Restrictions

Terrakube 2.34.0 adds workspace controls for where infrastructure changes can run and who can start them. Both settings are **off by default**, including after an upgrade. They are available in **Workspace > Settings > General**.

Only a superuser or a team with **Manage Workspaces at organization level** can change these settings. Project- or workspace-scoped management access is insufficient. This prevents a limited workspace manager from disabling the controls.

For a suggested production setup, see [Recommended Settings](../../getting-started/recommended-settings.md).

### Restrict apply and destroy to the configured branch

Enable **Only run apply and destroy from the default branch** (`restrictApplyToConfiguredBranch`) when infrastructure changes must come from the workspace's configured Git branch. Here, "default branch" means the branch configured in Terrakube, not necessarily the repository's default branch. If the workspace lists several branches, the first entry is used.

For example, with `main` configured, a developer can plan a feature branch, but must merge it into `main` before starting a run that applies it.

| Run source | Behavior with the restriction enabled |
| --- | --- |
| UI, REST or GraphQL on the configured branch | Apply/destroy templates are allowed. A blank branch override uses the configured branch. |
| UI, REST or GraphQL on another branch | Only plan-only templates are allowed. The run dialog filters the template choices. |
| UI/API run with an explicit `commitId` | Only plan-only templates are allowed, even if the branch matches. Remove the commit ID to run an apply/destroy template from the configured branch. |
| Push webhook or polled push | Apply/destroy templates are allowed only for the configured branch. Other branches can still use plan-only templates. |
| Pull request event or PR comment command | Only plan-only templates are allowed, even when the PR branch has the configured name: the PR could come from a fork. PR comment apply is disabled. |
| Release event | The event's template can start. Before apply/destroy, the executor verifies that the commit belongs to the configured branch. Protect release/tag publishing in your VCS too. |
| Terraform/OpenTofu CLI uploading configuration to a VCS/SSH workspace | Normal speculative plans work. Apply/destroy runs are refused because uploaded local configuration is not the configured branch. |
| Schedule or run trigger | Continues to use the workspace branch; the executor checks commit membership before apply/destroy. |

A plan-only template contains only `terraformPlan` or `terraformPlanDestroy` flow steps. A template with an approval, apply, destroy or custom-script flow step is not plan-only. Ensure the organization has a suitable plan-only template before enabling the restriction.

The executor checks that an apply/destroy commit is the configured branch tip or reachable from it. A release may therefore use an older commit on that branch. An API-created apply/destroy run with an explicit commit ID is still refused at creation; it does not reach this executor check. Branch and commit overrides cannot be edited after creation while the restriction is enabled.

The setting is disabled in the UI for CLI-driven workspaces, which receive uploaded configuration and have no repository branch to restrict. Use the approval and permission controls below for those workspaces.

{% hint style="warning" %}
Exact-commit CI workflows, including the documented [GitHub Action](../ci-cd-integration/github-actions.md) deployment workflow, send `commitId`. They cannot create apply-capable runs with this restriction enabled. Choose a branch-driven deployment workflow, or retain the exact-commit workflow with protected CI identities and approval steps.
{% endhint %}

### Require approval permission for applies without an approval step

Enable **Require the approve permission to start an apply with no approval step** (`requireApproveToApply`) to stop a user who can only plan from starting an apply through a template that has no preceding approval step.

| Template | Behavior with the setting enabled |
| --- | --- |
| Plan only | Plan permission is sufficient. |
| Plan, approval, apply | A planner can queue it; an authorized approver must release it. |
| Plan, apply without approval | The initiating caller also needs approve permission. |

This setting checks permission; it **does not add an approval step or require a different person to approve**. For a human review gate, use a template with an explicit approval step and an appropriate approval team. It covers UI/REST/GraphQL job creation and Terraform-compatible CLI run creation.

Terraform-compatible API runs requested with auto-approve require approve permission even when this setting is off, except speculative plans.

### Control who can destroy

The **Destroy runs** permission (`destroyJob`) is separate from plan and approve permissions in 2.34.0. It applies at organization, project and workspace levels; grants are additive across those levels.

* Starting a destroy with no preceding approval step requires plan and destroy permissions.
* A planner can queue a destroy behind an approval step. The approver also needs destroy permission.
* Auto-approving a CLI destroy removes the approval gate, so the initiating caller needs destroy permission as well as approve permission.

Admin and Write roles include destroy; Plan and Read do not. Custom access entries default `destroyJob` to **true**, including entries migrated during upgrade. Review existing custom teams and turn it off wherever destruction should require a different team.

This permission identifies destroy flow steps, including an apply following `terraformPlanDestroy`. It does not inspect an ordinary apply plan for resource deletions or replacements. Use [policies](../policy-enforcement-opa.md) to control those changes.

See [Team Management](../organizations/team-management.md), [Project Team Access](../projects/team-access.md) and [Workspace Team Access](team-access.md) for assigning access.
