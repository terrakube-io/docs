# Collection

This endpoint is used to manager collection inside an organization.

### Entity fields:

| Path                        | Type   | Description            |
| --------------------------- | ------ | ---------------------- |
| data.type                   | string | Should be "step"       |
| data.attributes.description | string | Collection description |
| data.attributes.name        | string | Collection name        |
| data.attributes.priority    | int    | Collection priority    |

### Example:

```json
POST /api/v1/organization/${ORGANIZATION_ID}/collection/

{
    "data": {
        "type": "collection",
        "attributes": {
            "description": "Sample Description",
            "name": "Collection1",
            "priority": 10,
        }
}

```

### Supported Operation

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/collection" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151108Z&X-Amz-Expires=172800&X-Amz-Signature=9fa2b1a9f65e3f54965706273602d3e4c477394bb42bea56fdf3e331e85e2595&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/collection" method="post" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151108Z&X-Amz-Expires=172800&X-Amz-Signature=9fa2b1a9f65e3f54965706273602d3e4c477394bb42bea56fdf3e331e85e2595&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/collection/{collectionId}" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151108Z&X-Amz-Expires=172800&X-Amz-Signature=9fa2b1a9f65e3f54965706273602d3e4c477394bb42bea56fdf3e331e85e2595&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/collection/{collectionId}" method="delete" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151108Z&X-Amz-Expires=172800&X-Amz-Signature=9fa2b1a9f65e3f54965706273602d3e4c477394bb42bea56fdf3e331e85e2595&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/organization/{organizationId}/collection/{collectionId}" method="patch" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151108Z&X-Amz-Expires=172800&X-Amz-Signature=9fa2b1a9f65e3f54965706273602d3e4c477394bb42bea56fdf3e331e85e2595&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}
