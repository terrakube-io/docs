# Agent

This endpoint is used to create, update, search or delete job agents for a Terrakube organization.

{% hint style="info" %}
You need to be part of the administrator group to work with this endpoint
{% endhint %}

### Entity fields:

| Path                        | Type   | Description                                                                                                                                                                    |
| --------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| data.type                   | string | Should be "agent"                                                                                                                                                              |
| data.attributes.name        | string | Agent pool name                                                                                                                                                                |
| data.attributes.description | string | agent pool description                                                                                                                                                         |
| data.attributes.url         | string | URL where the executor component will be available.  Example: [http://terrakube-executor-service.self-hosted-executor](http://terrakube-executor-service.self-hosted-executor) |

### Example:

```
POST {{terrakubeApi}}/api/v1/organization/{{organizationId}}/agent
{
  "data": {
    "type": "agent",
    "attributes": {
      "name": "sample-agent",
      "url": "http://localhost:8090",
      "description": "This is a sample agent"
    }
  }
}
```

### Supported Operations:

####

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/agent" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/agent" method="post" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/agent/{agentId}" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/agent/{agentId}" method="delete" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/agent/{agentId}" method="patch" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}
