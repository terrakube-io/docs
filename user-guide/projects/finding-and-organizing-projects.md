# Finding and Organizing Projects

From Terrakube 2.35 the projects list shows the health of each project and stays fast in organizations with thousands of projects. Searching, filtering, sorting and paging happen on the server, so only the page you are looking at is loaded.

### The projects list

Under 25 projects the list opens as cards, above that as a table. Your choice of **Cards** or **Table** is remembered in your browser.

Each project shows its workspace count, a health bar (completed, failed, running, queued, waiting for approval, never run) and the time of its last run. A project **needs attention** when a workspace failed, is waiting for approval or is not compliant with policy. Locked workspaces are not counted, because a lock is usually deliberate.

The chips under the search box filter the list and show how many projects each matches:

| Chip | Shows |
| --- | --- |
| All | every project you can see |
| Needs attention | projects with failed, waiting or non-compliant workspaces |
| My projects | projects one of your teams has been given access to |
| Empty | projects with no workspaces |
| Stale (90d) | projects whose last run was more than 90 days ago |

The **Policy** column rolls up the latest policy result of each workspace: "N hard" when any workspace has a hard mandatory violation, otherwise "N soft", "Compliant", or "Not evaluated". Workspaces evaluated before 2.35 have no hard or soft split and show as "N non-compliant" until they run again. If a count cannot be loaded it shows a dash with a **Retry** button, never 0.

Workspaces that are in no project appear as a dashed **Unassigned** entry with a link to list them. You only see projects you can access: those in organizations where your team can manage workspaces, and those your team has been given access to.

### Links to a view

The list and a project's workspace list keep their search, filters, sort and page in the address, so you can bookmark a view or send the link. For example `.../projects?chip=attention` lists the projects that need attention, and `.../projects/<id>/workspaces?status=failed` lists a project's failed workspaces. Values the page does not recognise open the default view.

Every number leads to what it counts. On the Overview, the status rows and each state in the health legend open the project's workspaces filtered to that state. Press `/` outside a text field to jump to the search box.

### The project page

The left menu inside a project has **Overview**, **Workspaces (n)**, **Policies**, **Team access**, **Variables** and **Settings**. **New workspace** creates a workspace in the project.

The header shows **your access** as a chip, for example "Write, via payments-devs". Open it to see what you can do in the project (view, plan, approve, manage workspaces, manage state, manage policies, edit and delete the project). A banner appears only if you cannot change the project, and says what is blocked.

The **Overview** shows:

* **Project health**: a ring of the workspaces by state, how many run from a repository (VCS) and how many from the CLI or API, and how many are locked. A CLI workspace on local execution only uploads its state, so Terrakube has no run status for it. It is counted as "no run status", not "never executed".
* **Failed runs**, **Waiting approval** and **Policy violations**, each with a link to the workspaces.
* **Needs attention** and **Recent activity**, then the delivery numbers for the last 30 days and the project's details.

The tab a project opens on is chosen at three levels, and the first one set wins:

1. **You**: **Open this project on** at the bottom of the Overview, remembered in your browser.
2. **The project**: **Opens on** under the project's **Settings**.
3. **The organization**: **Project landing tab** under **General settings**.

**Policies** lists the policy sets that apply to every workspace in the project: global sets and sets attached to the project. **Team access** lists the teams with access, or says so if you cannot see them.

### Workspaces

The **Workspaces** tab has the same filters as the organization's workspace list: search, status, policy and tags. Tags that came from the project's default tags are marked "from project".

Tick workspaces to act on several at once. The bar offers **Plan**, **Approve**, **Add tag**, **Move to project**, **Lock**, **Unlock** and **Remove from project**, and applies to the current page only.

* **Plan** starts one run per selected workspace from a template you choose, on the workspace's own branch. A template that also applies will apply on every selected workspace, subject to each workspace's approval rules, so choose a plan-only template to preview changes.
* **Approve** lists the runs waiting for approval and approves them when you confirm. Each workspace is changed separately, and the result says how many succeeded and why any failed.

### Moving workspaces

The move dialog shows what changes before you confirm: teams that gain or lose access, policy sets and variable collections that start or stop applying, and default tags the workspaces inherit. Access granted directly on a workspace is kept.

### Default tags

Tags under **General** > **Default tags** are copied onto workspaces created in the project or moved into it. They are copied once:

* workspaces already in the project are not changed unless you choose **Apply to existing workspaces**
* a tag the workspace already has keeps its own value
* removing a default tag does not remove it from any workspace

**Apply to existing workspaces** shows how many tags would be added to how many workspaces and changes nothing until you confirm. It only adds missing tags, so running it again changes nothing. Scripts can preview the same way:

```
POST /access-token/v1/projects/organization/{organizationId}/project/{projectId}/default-tags/apply?dryRun=true
```

Pass `dryRun=false` to apply. The caller needs to manage the project; anyone else gets `404`.

### Deleting a project

**Delete Project** under Settings shows what the project contains, which policy attachments and exemptions would be removed, which variable collections would be detached, and how many default tags would go. Type the project name to confirm. If the project still has workspaces, the button reads **Move to Unassigned, then delete**: the workspaces are moved out first, not deleted.

The API enforces the same rule: deleting a project that still has workspaces, variable collections, policy attachments or policy exemptions answers `409` and says how many are in the way.

### Settings

* **Owner**: the team that owns the project, as a team name. It is shown in lists and in the inbox, and people can limit their inbox to their own teams. It does not grant access.
* **Defaults for new workspaces**: a Terraform version, an execution mode and a template, applied when a workspace is created in the project without a value. Each is labelled "Defaulted from project" or "Defaulted from organisation".
* **Variables**: the variable collections attached to the project. See [Variable Collections](../organizations/variable-collections.md).
* **Effective variables**: choose a workspace to see the variables its runs will use and where each comes from. Sensitive values are never shown.

### Permissions

The interface reads your permissions on a project from the API, which combines your organization team and any project access your teams have:

```
GET /access-token/v1/teams/permissions/organization/{organizationId}/project/{projectId}
```

It returns only the caller's own permissions, including `manageProject`, with the teams that grant them (`grantedThrough`) and the highest role among them (`accessLevel`). It answers `404` when the project is not in that organization. The organization and workspace permission endpoints return the same two fields.
