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

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/workspace/{workspaceId}/variable/{variableId}" method="get" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/workspace/{workspaceId}/variable" method="post" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/workspace/{workspaceId}/vcs/{vcsId}" method="delete" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v1_4.yml" path="/workspace/{workspaceId}/vcs/{vcsId}" method="patch" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [Open API specification.](https://editor.swagger.io/?url=https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1\_4.yml)
{% endhint %}
