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

{% openapi-operation spec="terrakube-api" path="/workspace/{workspaceId}/variable/{variableId}" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi src="../../.gitbook/assets/v1_4.yml" path="/workspace/{workspaceId}/variable" method="post" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v1_4.yml" path="/workspace/{workspaceId}/vcs/{vcsId}" method="delete" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v1_4.yml" path="/workspace/{workspaceId}/vcs/{vcsId}" method="patch" %}
[v1_4.yml](../../.gitbook/assets/v1_4.yml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
