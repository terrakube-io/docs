# History

This endpoint is used to see terraform states changes over time for a Terrakube workspace.

{% hint style="warning" %}
To use this endpoint you should have "manageWorkspace" access at team level and a Terrakube workspace
{% endhint %}

### Entity fields:

| Path                   | Type   | Description          |
| ---------------------- | ------ | -------------------- |
| data.type              | string | Should be "history"  |
| data.attributes.output | string | Unique variable name |

### Example:

```
{
  "data": {
    "type": "history",
    "attributes": {
      "output": "Terraform JSON State URL"
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/workspace/{workspaceId}/history" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
