# Schedule

This endpoint is used to create, update, search or delete schedules for a particular workspace inside an organization, this is useful when you need to create schedule task or running jobs in a particular time.

### Entity fields:

| Path                              | Type   | Description                                                                                                                                                                                                   |
| --------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| data.type                         | string | Should be "template"                                                                                                                                                                                          |
| data.attributes.cron              | string | Cron expression to schedule a job inside a workspace. For more information please visit [Quartz documentation](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/tutorial-lesson-06.html). |
| data.attributes.tcl               | string | Terrakube configuration language in base64                                                                                                                                                                    |
| data.attributes.templateReference | string | Terrakube template id                                                                                                                                                                                         |

{% hint style="info" %}
To better understand Terrakube Configuration Language job templates please refer to the following [GitHub repository.](https://github.com/AzBuilder/terrakube-extensions)
{% endhint %}

### Example:

```
{
  "data": {
    "type": "schedule",
    "attributes": {
      "cron": "0 0/1 * * * ?",
      "tcl": "{{templateSample}}"
    }
  }
}
```

### Supported Operations

{% openapi-operation spec="terrakube-api" path="/workspace/{workspaceId}/schedule" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/workspace/{workspaceId}/schedule" method="post" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="delete" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="patch" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% openapi-operation spec="terrakube-api" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="get" %}
[OpenAPI terrakube-api](https://gitbook-x-prod-openapi.4401d86825a13bf607936cc3a9f3897a.r2.cloudflarestorage.com/raw/cb9b759d0a5961decbc26bf547567df170cc7d69216b1d84e1fdc034f053cda2.json?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=dce48141f43c0191a2ad043a6888781c%2F20250804%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250804T151110Z&X-Amz-Expires=172800&X-Amz-Signature=29e4501ef70743026361a7df2e6b1059bdfc5099b76ccb37ab41bde96753e35a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
{% endopenapi-operation %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
