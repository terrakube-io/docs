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
| data.attributes.description | string | Agent pool description                                                                                                                                                         |
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

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/agent" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/agent" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/agent/{agentId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/agent/{agentId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/agent/{agentId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}
