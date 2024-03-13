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

### Workspace Operations:

#### Update workspace to use specific agent.

```
PATCH {{terrakubeApi}}/api/v1/organization/{{organizationId}}/workspace/{{workspaceId}}/relationships/agent
{
  "data": {
      "type": "agent",
      "id": "f0ddb2e5-c390-4451-bd7f-fe84e74010de"
  }
}
```

Remove agent from workspace

```
PATCH {{terrakubeApi}}/api/v1/organization/{{organizationId}}/workspace/{{workspaceId}}/relationships/agent
{
  "data":  null
}
```
