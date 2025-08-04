# Step

This endpoint is used to see the output for a job step.

### Entity fields:

| Path                       | Type   | Description                                                                              |
| -------------------------- | ------ | ---------------------------------------------------------------------------------------- |
| data.type                  | string | Should be "step"                                                                         |
| data.attributes.output     | string | URL with the output for the step                                                         |
| data.attributes.status     | string | Step status(pending or completed)                                                        |
| data.attributes.stepNumber | int    | A job can have several steps, this field is used to identity the order for the execution |

### Example:

```
{
  "data": {
    "type": "step",
    "attributes": {
      "output": "URL with the step output",
      "stepNumber": "Step Number Order",
      "status": "Terraform Provider"
    }
  }
}
```

### Supported Operations

{% openapi-operation spec="terrakube-api" path="/step/{stepId}" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151109Z&X-Amz-Expires=172800&X-Amz-Signature=b8e5244069f0315750515e4bb8cf20ae1917938f41a4c0e65a72ac76f51b0751&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
