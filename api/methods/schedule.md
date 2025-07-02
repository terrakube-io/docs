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

{% swagger src="https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="get" %}
[https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml](https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml" path="/workspace/{workspaceId}/schedule" method="post" %}
[https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml](https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="delete" %}
[https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml](https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml)
{% endswagger %}

{% swagger src="https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml" path="/workspace/{workspaceId}/schedule/{scheduleId}" method="patch" %}
[https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml](https://raw.githubusercontent.com/AzBuilder/terrakube-server/main/openapi-spec/v1_6.yml)
{% endswagger %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
