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

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/workspace/{workspaceId}/schedule" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/workspace/{workspaceId}/schedule" method="post" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="delete" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="patch" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_6.yaml" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
