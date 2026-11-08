# Finding and Organizing Projects

From Terrakube 2.35 the projects list shows the health of each project at a glance and stays fast in organizations with thousands of projects.

### The projects list

Under 25 projects the list opens as cards, above that as a table. Your choice (**Cards** or **Table**) is remembered in your browser and wins over that default.

Each project shows its number of workspaces, a health bar and the time of its last run. The health bar splits the project's workspaces into completed, failed, running, queued (hatched), waiting for approval and never run. The key is shown under the list.

A project **needs attention** when it has workspaces that failed, are waiting for approval, or are not compliant with policy. Locked workspaces are not counted as needing attention, because a lock is usually deliberate. A card shows a red "N need attention" badge for failed or waiting runs, and an amber "N non-compliant" badge when policy is the only issue.

The chips under the search box filter the list on the server, and each shows how many projects it matches:

| Chip | Shows |
| --- | --- |
| All | every project you can see |
| Needs attention | projects with failed, waiting or non-compliant workspaces |
| My projects | projects one of your teams has been given access to |
| Empty | projects with no workspaces |
| Stale (90d) | projects whose last run was more than 90 days ago |

Searching, sorting and paging also happen on the server, so only the page you are looking at is loaded, however many projects there are. In the table every column header sorts. If a count cannot be loaded it is shown as a dash with a **Retry** button, never as 0.

Workspaces that are in no project appear as a dashed **Unassigned** card, or an italic row in the table, with a link to move them into a project.

The **Policy** column rolls up the latest policy evaluation of each workspace: "N hard" when any workspace has a hard mandatory violation, otherwise "N soft" for soft mandatory violations, "Compliant", or "Not evaluated" when no workspace has been evaluated yet. Workspaces evaluated before 2.35 have no hard or soft split and are shown as "N non-compliant" until they run again.

If you cannot see who has access to projects (you do not have the Manage Workspaces permission), the Teams slot shows a dash, and hovering it gives the total number of teams.

You only see the projects you can access: projects in organizations where your team can manage workspaces, and projects your team has been given access to.

### Sharing a view and getting to the detail

The projects list and a project's workspace list keep their search, filters, sort and page in the address, so you can bookmark a view, send the link to a colleague, and come back to it with the browser's back button. For example `.../projects?chip=attention` lists the projects that need attention, and `.../projects/<id>/workspaces?status=failed` lists a project's failed workspaces. A link with values the page does not recognise opens the default view instead of failing.

Every number leads to what it counts. On the list, the attention count and the policy roll-up open the project's workspaces filtered to the problem. On a project's Overview, each tile and each status in the health legend does the same, and the status next to a workspace in "Needs attention" or "Recent activity" opens that workspace's runs, which is where a failure or a pending approval is read. "Needs attention" mixes failed runs, runs waiting for approval and policy violations, which one filter cannot show together, so it opens the worst one present (failed, then waiting, then non-compliant).

Press `/` anywhere outside a text field to jump to the search box on the projects list.

When a control is disabled, hovering or focusing it says what is missing, for example "Creating a workspace here needs write access to this project, or Manage Workspaces in the organization."

### The project page

Inside a project the left menu changes to **Back to projects**, **Overview**, **Workspaces (n)**, **Policies**, **Team access** and **Settings**. A banner under the title says what you can do here, for example "Your access in this project: Write, through team payments-devs. You cannot manage access or delete the project." **Edit** opens Settings and **New workspace** creates a workspace in the project.

The **Overview** shows four tiles (workspaces, need attention, queued now with the longest wait, and teams with access), the health bar with the count of each status, the workspaces that need attention, and recent activity.

The tab a project opens on can be set at three levels, and the first one that is set wins:

1. **You**: **Open this project on** at the bottom of the Overview. It is remembered in your browser and applies to every project. Choose **Project default** to forget it.
2. **The project**: **Opens on** under the project's **Settings**.
3. **The organization**: **Project landing tab** under the organization's **General settings**.

When none is set, projects open on the Overview.

**Policies** lists the policy sets that apply to every workspace in the project: global sets, and sets attached to the project itself. **Team access** lists the teams with access. If your own access does not let you see who has access, the page says so instead of showing an empty list.

### Workspaces

The **Workspaces** tab uses the same filters as the organization's workspace list: search, status, policy and tags. Tags that came from the project's default tags are marked "from project". Rows can also be shown as cards.

Tick workspaces to act on several at once. The bar says how many are selected and that the selection covers the current page only, and offers **Plan**, **Approve**, **Add tag**, **Move to project**, **Lock** or **Unlock**, and **Remove from project**.

* **Plan** starts one run on each selected workspace from a template you choose, on the workspace's own branch. Choose a plan-only template to preview changes: a template that also applies will apply on every selected workspace, subject to the approval each workspace requires.
* **Approve** is available when a selected workspace has a run waiting for approval. It lists the runs it found and approves them once you confirm. Each workspace is changed separately and a failure on one does not stop the others; the result message tells you how many succeeded and why any failed. An empty project offers **New workspace** and **Move existing**.

### Moving workspaces

Before a move is confirmed, the dialog shows what it changes: which teams gain or lose access, which policy sets start or stop applying, which variable collections start or stop applying, and which default tags the workspaces will inherit. Access granted directly on a workspace is kept. If you cannot see who has access to a project, the dialog says so instead of guessing.

### Default tags

Tags set under **General** > **Default tags** are copied onto workspaces that are created in the project or moved into it. They are copied once, so:

* workspaces already in the project are not changed when you add a default tag, unless you choose **Apply to existing workspaces**
* a tag the workspace already has keeps its own value
* removing a default tag does not remove it from any workspace

**Apply to existing workspaces** first shows how many tags would be added to how many workspaces, and only changes anything when you confirm. It adds only tags a workspace is missing, so running it again changes nothing. Scripts can use the same preview:

```
POST /access-token/v1/projects/organization/{organizationId}/project/{projectId}/default-tags/apply?dryRun=true
```

Pass `dryRun=false` to apply. The caller needs to manage the project; anyone else gets `404`.

Because the tags are real workspace tags, tag filters and tag-based policy sets behave exactly as before.

### Deleting a project

**Delete Project** under Settings opens a pre-flight that says what the project contains (workspaces and team access rows), which policy attachments and exemptions would be removed, which project-scoped variable collections would be detached, and how many default tags would go. Type the project name to confirm. If the project still has workspaces the button reads **Move to Unassigned, then delete**: the workspaces are moved out first (they are not deleted), then the policy attachments, exemptions and collection attachments are removed, then the project.

The server enforces the same rule for scripts and the API. Deleting a project that still has workspaces, variable collections, policy attachments or policy exemptions answers `409` and says how many are in the way.

### Configuration

**Settings** also holds the project's configuration:

* **Defaults for new workspaces**: a Terraform version, an execution mode and a template. They are applied when a workspace is created in the project without a value, and the create form prefills the version. Each is labelled "Defaulted from project" or "Defaulted from organisation".
* **Variables**: the variable collections attached to the project. See [Variable Collections](../organizations/variable-collections.md).
* **Effective variables**: choose a workspace to see the variables its runs will use and where each comes from (the workspace, the organization's global variables, or a collection and its scope). Sensitive values are never shown. Each source links to where it is edited.

### Permissions

The interface reads your permissions on a project from the API, which combines your organization team and any project access your teams have. The same view is available to scripts:

```
GET /access-token/v1/teams/permissions/organization/{organizationId}/project/{projectId}
```

It returns only the caller's own permissions, including `manageProject`, and answers `404` when the project is not in that organization.
