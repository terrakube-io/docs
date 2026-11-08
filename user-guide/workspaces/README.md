# Workspaces



When working with Terraform at Enterprise level, you need to organize your infrastructure in different collections. Terrakube manages infrastructure collections with **workspaces**. A workspace contains everything Terraform needs to manage a given collection of infrastructure, so you can easily organize all your resources based in your requirements.

For example, you can create a workspace for dev environment and a different workspace for production. Or you can separate your workspaces based in your resource types, so you can create a workspace for all your SQL Databases and anothers workspace for all your VMS.

<figure><img src="../../.gitbook/assets/Untitled.drawio.png" alt=""><figcaption></figcaption></figure>

You can create unlimited workspaces inside each Terrakube Organization.

### Workspace setup order

Choose your workflow and controls in [Recommended Settings](../../getting-started/recommended-settings.md) first. The full deployment-to-run sequence is in [Production Setup](../../getting-started/production-setup.md).

1. [Create the workspace](creating-workspaces.md) with its project, source and branch or CLI workflow.
2. Assign [Team Access](team-access.md), add [variables](variables.md), and configure [dynamic provider credentials](dynamic-provider-credentials/README.md).
3. Apply [Run Restrictions](run-restrictions.md) in **Settings > General**. These settings are off by default and require an organization-level workspace manager.
4. Check the [CLI](cli-driven-workflow.md), [API](api-driven-workflow.md) or [CI](../ci-cd-integration/README.md) guide for your chosen workflow, then [start a run](starting-a-run.md) and verify its approval path.
5. Add [webhooks](webhooks.md), [schedules](workspace-scheduler.md) or [run triggers](run-triggers.md) after the run works with the intended permissions.

### Workspaces list

<figure><img src="../../.gitbook/assets/workspaces-list-grouped.png" alt="The Workspaces list in Compact view, grouped by project"><figcaption><p>The Workspaces list, grouped by project</p></figcaption></figure>

The **Workspaces** page has two views, toggled in the top right and remembered per browser:

* **New** — a condensed list with click-to-filter tags and project chips, an optional **group by project** mode (each project paginates independently), and a lock indicator per workspace.
* **Legacy** — the original table view.

Use the search box and tag/project filters to narrow a long list; clicking a tag or project chip on any row applies it as a filter directly.

#### Tags in the list

Each tag is shown as a chip with its key and, for a key/value tag, its value (hover a chip to see `key = value`). Tags are sorted by key, then by value. The **Cards** view shows every tag on each card.

<figure><img src="../../.gitbook/assets/workspaces-list-cards-tags.png" alt=""><figcaption></figcaption></figure>

In the **Compact** view, a tag icon next to the workspace name opens all of the workspace's tags in a popover on hover, click or keyboard focus.

<figure><img src="../../.gitbook/assets/workspaces-list-tag-popover.png" alt=""><figcaption></figcaption></figure>

#### Filtering by tag

Click **Tags** in the filter bar to open the tag filter. It takes one or more rows, each with a **tag key** and an optional **value**; click **Filter by another tag** to add a row:

* A row with a value matches workspaces where that key has exactly that value, for example `env = development`.
* A row without a value matches every workspace that has the key, whatever its value.
* Several rows are combined with AND, so `env = development` and `team = platform` matches only the workspaces that have both.

Click **Apply filter** to apply the rows, or **Clear** in the tag filter popover to remove just the tag filters.

<figure><img src="../../.gitbook/assets/workspaces-tag-filter.png" alt=""><figcaption></figcaption></figure>

### In this section

**Set up**: [Creating Workspaces](creating-workspaces.md), [Overview](overview.md), [Variables](variables.md), [Team Access](team-access.md)

**Run**: [Starting a Run](starting-a-run.md), [Run Restrictions](run-restrictions.md), [Webhooks](webhooks.md), [Workspace scheduler](workspace-scheduler.md), [Run Triggers](run-triggers.md), [Job Output](job-output.md), [Notifications](notifications.md)

**Workflows**: [CLI-driven](cli-driven-workflow.md), [API-driven](api-driven-workflow.md), [Ephemeral Workspaces](ephemeral-workspaces.md)

**State and caching**: [Terraform State](terraform-state.md), [Share Workspace State](share-workspace-state.md), [Provider Cache](provider-cache.md), [Module Cache](module-cache.md), [Plan Artifacts](plan-artifacts.md)

**Credentials and extensions**: [Dynamic Provider Credentials](dynamic-provider-credentials/README.md), [Actions](actions/README.md)
