# History

This endpoint is used to see terraform states changes over time for a Terrakube workspace.&#x20;

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

{% swagger src="../../.gitbook/assets/v1_5.yml" path="/workspace/{workspaceId}/history" method="get" %}
[v1_5.yml](../../.gitbook/assets/v1_5.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/blob/main/openapi-spec/v1\_6.yml)
{% endhint %}
