# Run Trigger

Links two workspaces so that an apply in the source queues a run in the destination. See [Run Triggers](../../user-guide/workspaces/run-triggers.md).

{% hint style="warning" %}
Requires manage rights on the destination workspace and visibility of the source workspace.
{% endhint %}

### Entity fields:

| Path | Type | Description |
| --- | --- | --- |
| data.type | string | Should be "runTrigger" |
| data.attributes.enabled | boolean | Disabled triggers are kept but never fire (default true) |
| data.relationships.sourceWorkspace.data | object | Workspace whose apply fires the trigger. Cannot be changed after creation |
| data.relationships.destinationWorkspace.data | object | Workspace that receives the run. Cannot be changed after creation |
| data.relationships.template.data | object | Template to run in the destination (OPTIONAL, defaults to the workspace's default template) |

### Example:

```json
{
  "data": {
    "type": "runTrigger",
    "attributes": { "enabled": true },
    "relationships": {
      "sourceWorkspace": { "data": { "type": "workspace", "id": "{{sourceWorkspaceId}}" } },
      "destinationWorkspace": { "data": { "type": "workspace", "id": "{{destinationWorkspaceId}}" } }
    }
  }
}
```

A trigger that would create a loop is rejected with the offending path in the error. To list both directions for a workspace, filter with `sourceWorkspace.id==X,destinationWorkspace.id==X` (a comma is OR).
