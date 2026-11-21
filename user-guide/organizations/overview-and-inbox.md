# Overview, Inbox and Quick Navigation

From Terrakube 2.35 an organization opens on an **Overview**, and what needs a person is reachable from every page.

### Organization overview

The overview answers three questions in order: how are the workspaces, what needs me, and how are the projects.

* **Workspace health**: a ring of all workspaces by the state of their last run, and the share that are compliant with policy. Next to it, **Failed runs**, **Waiting approval** and **Drifted** link to those workspaces.
* **Needs you** and **Recent runs**: the first items from the inbox (waiting for approval, failed, drifted or non-compliant) with **Approve** inline, and the latest runs.
* **Projects**: the projects that need attention most, with a note of how many more there are and a link to all of them.

### Inbox

The inbox at `/inbox` lists what needs attention, one organization at a time, in four groups with their counts:

| Group | Contains |
| --- | --- |
| Waiting for approval | runs waiting for an approval |
| Failed runs | workspaces whose last run failed |
| Drifted | workspaces where a scheduled plan found changes, see [Drift Detection](../drift-detection.md) |
| Policy violations | workspaces that are not compliant with policy |

It opens on the first group that has something. Each group is a table of 25 rows a page, with the reason and how long ago, the project and its owner team, and the next step. The toolbar has an organization picker, a search by workspace name and a switch between **Everything I can see** and **My teams**. "My teams" limits the list to projects whose [owner](../projects/finding-and-organizing-projects.md#settings) is one of your teams.

To approve several runs, tick them under **Waiting for approval** and choose **Approve**. After one confirmation each run is approved separately, and runs that are no longer waiting are counted and skipped. Without an organization picked, the inbox shows one row per organization with its counts, and each number opens that group.

The organization, group, search, page and scope are in the address, so a link, or the back button from a workspace, returns to the same list.

### The bell

The bell in the sidebar header shows how many things need attention in the organization you are working in (99+ for large numbers). The count refreshes every minute and only reads that one organization. Opening it lists the first six with **Approve** inline, a **View all** link to that organization in the inbox, and the other organizations with their counts. With more than 20 organizations the others are listed without counts. If the inbox cannot be read, the bell says so.

### Search and shortcuts

Press `Cmd+K` (`Ctrl+K` on Windows and Linux), or choose **Search** in the sidebar, to go to a page, organization, project or workspace by typing part of its name. It also lists the pages you opened last. On a workspace, the palette offers **Approve run** when that workspace has a run waiting.

Outside a text field, `g` then a letter moves between pages: `g i` the inbox, `g p` the projects and `g w` the workspaces of the current organization.

### Links by name

`/go` links open a page from names, so a repository, a pull request comment or a portal can link to Terrakube without knowing ids:

```
/go/<organization>
/go/<organization>/projects/<project>
/go/<organization>/workspaces/<workspace>
/go/<organization>/workspaces/<workspace>/<section>
/go/<organization>/workspaces/<workspace>/runs/<run number>
```

`<section>` is one of `runs`, `states`, `variables`, `schedules`, `run-triggers` or `settings`. Anything else is refused, and a name that does not exist shows what was not found.

### Managed in code

If the organization is kept in Terraform, set **Managed in code** under **General settings** with the repository, so people know that edits made in the interface are reverted by the next apply. The organization's General settings, the project page and the workspace settings then show a "Managed by Terraform" notice with a link to the repository. With **Show a notice and make them read-only**, project and workspace settings cannot be edited in the interface. This only changes what the screens say and allow: it never blocks running, approving or locking, and the API does not enforce it.
