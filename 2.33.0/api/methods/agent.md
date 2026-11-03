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

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/agent" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/agent" method="post" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/agent/{agentId}" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/agent/{agentId}" method="delete" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/agent/{agentId}" method="patch" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}
