# Finding and Organizing Projects

From Terrakube 2.35 the projects list shows the health of each project at a glance and stays fast in organizations with thousands of projects.

### The projects list

Each project shows its number of workspaces, a health bar and the time of its last run. The health bar splits the project's workspaces into failed, waiting for approval, running or queued, applied, and never run.

A project **needs attention** when it has workspaces that failed, are waiting for approval, or are not compliant with policy. Locked workspaces are not counted as needing attention, because a lock is usually deliberate.

Searching, sorting and paging happen on the server, so only the page you are looking at is loaded. You can:

* search by name or description
* sort by needs attention (the default), name, last run, or number of workspaces
* show only the projects that need attention
* switch between a table and cards; the choice is remembered in your browser

Workspaces that are not in any project are mentioned under the list, with a link to the workspaces page.

You only see the projects you can access: projects in organizations where your team can manage workspaces, and projects your team has been given access to.

### The project page

A project opens on its **Overview**, which shows the number of workspaces, how many are running or queued and how many are locked, the health bar, and an alert when workspaces need attention.

The tab a project opens on can be changed with **Open projects on** at the bottom of the Overview. The choice is remembered in your browser.

The other tabs are:

| Tab | Purpose |
| --- | --- |
| Workspaces | The project's workspaces. Select several to move them to another project or remove them from the project. |
| Variables | Variable collections that apply to every workspace in the project. See [Variable Collections](../organizations/variable-collections.md). |
| Teams | Team access to the project. |
| General | Name, description, default tags and deleting the project. |

### Moving workspaces in bulk

On the **Workspaces** tab, tick the workspaces and choose **Move to project** or **Remove from project**. Each workspace is changed separately and a failure on one does not stop the others; the result message tells you how many succeeded and why any failed.

Moving a workspace changes who can access it: teams on the old project lose project-level access to it and teams on the destination project gain it. Access granted directly on the workspace is kept.

### Default tags

Tags set under **General** > **Default tags** are copied onto workspaces that are created in the project or moved into it. They are copied once, so:

* workspaces already in the project are not changed when you add a default tag
* a tag the workspace already has keeps its own value
* removing a default tag does not remove it from any workspace

Because the tags are real workspace tags, tag filters and tag-based policy sets behave exactly as before.

### Deleting a project

A project cannot be deleted while it still has workspaces or attached variable collections. The error tells you how many are in the way. Move or delete the workspaces, and detach the collections, then delete the project.

### Permissions

The interface reads your permissions on a project from the API, which combines your organization team and any project access your teams have. The same view is available to scripts:

```
GET /access-token/v1/teams/permissions/organization/{organizationId}/project/{projectId}
```

It returns only the caller's own permissions, including `manageProject`, and answers `404` when the project is not in that organization.
