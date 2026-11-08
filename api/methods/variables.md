# Variables

This endpoint is used to create, update, search or delete variables information inside a Terrakube workspace.

{% hint style="warning" %}
To use this endpoint you should have "manageWorkspace" access at team level and a Terrakube workspace
{% endhint %}

### Entity fields:

| Path                        | Type    | Description                                                    |
| --------------------------- | ------- | -------------------------------------------------------------- |
| data.type                   | string  | Should be "variable"                                           |
| data.attributes.key         | string  | Unique variable name                                           |
| data.attributes.value       | string  | Key value                                                      |
| data.attributes.sensitive   | boolean | To hide the value when the output is sensitive                 |
| data.attributes.hcl         | boolean | Terraform HCL variable type                                    |
| data.attributes.category    | string  | Variable type could be TERRAFORM or ENV (Environment Variable) |
| data.attributes.description | string  | Free text                                                      |

### Example:

```
{
  "data": {
    "type": "variable",
    "attributes": {
      "key": "tag_name",
      "value": "HolaMundo",
      "sensitive": false,
      "hcl": false,
      "category": "TERRAFORM",
      "description": "Azure RG Tag"
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/workspace/{workspaceId}/variable/{variableId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/workspace/{workspaceId}/variable" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/workspace/{workspaceId}/vcs/{vcsId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/workspace/{workspaceId}/vcs/{vcsId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
