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

{% openapi-operation spec="terrakube-api" path="/workspace/{workspaceId}/history" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
