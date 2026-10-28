# Run Triggers

A run trigger links two workspaces in the same organization: when the **source** workspace finishes a run that changed infrastructure, Terrakube queues a run in the **destination** workspace. Use it when one workspace consumes the outputs of another, for example a VPN workspace that reads a VPC workspace's outputs, so the dependent workspace does not silently drift.

{% hint style="info" %}
Available from Terrakube 2.34.0.
{% endhint %}

### Configure a trigger

Open the destination workspace and select **Run Triggers** in the workspace menu.

* **Runs after** lists the workspaces whose applies start a run here. Add a source workspace, optionally choose a template (otherwise the destination's default template is used), and toggle a trigger on or off.
* **Triggers** lists the workspaces that run after this one. It is read-only here; change those from the other workspace.

Runs started this way say so in the run header and link to the upstream run.

### What fires a trigger

* Only a run that **applied** (or destroyed) changes. A plan-only run, or an apply with no changes, fires nothing.
* If the destination is locked or busy, its run is queued rather than skipped.
* Triggers cascade: a destination can itself be the source of other triggers.

### Rules

* You need manage rights on the destination workspace, and must be able to see the source.
* Terrakube rejects a trigger that would create a loop, and names the path in the error.
* The two workspaces of a trigger cannot be changed once created. Delete it and create another.

### Limits

Two safety bounds, set on the API:

| Property | Default | Meaning |
| --- | --- | --- |
| `io.terrakube.run-trigger.max-cascade-depth` | `10` | Longest chain of runs started by one apply. |
| `io.terrakube.run-trigger.max-dependents-per-apply` | `20` | Most workspaces one apply can trigger. |
| `io.terrakube.run-trigger.enabled` | `true` | Turns run triggers off for the whole instance. |

Triggers are also available over the API as the `runTrigger` resource (`sourceWorkspace`, `destinationWorkspace`, `template`, `enabled`).
