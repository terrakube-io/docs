# Globalvar

This endpoint is used to create, update, search or delete global variables information inside a Terrakube organizations, this variables will be injected inside the jobs, workspace variables have priority over global variables if the same name is used.

{% hint style="warning" %}
You need to be part of the administrator Active Directory Group to work with this endpoint
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
    "type": "globalvar",
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

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/globalvar" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151109Z&X-Amz-Expires=172800&X-Amz-Signature=b8e5244069f0315750515e4bb8cf20ae1917938f41a4c0e65a72ac76f51b0751&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi src="../../.gitbook/assets/v2_4.yaml" path="/organization/{organizationId}/globalvar" method="post" %}
[v2_4.yaml](../../.gitbook/assets/v2_4.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_4.yaml" path="/organization/{organizationId}/globalvar/{globalvarId}" method="delete" %}
[v2_4.yaml](../../.gitbook/assets/v2_4.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_4.yaml" path="/organization/{organizationId}/globalvar/{globalvarId}" method="patch" %}
[v2_4.yaml](../../.gitbook/assets/v2_4.yaml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
