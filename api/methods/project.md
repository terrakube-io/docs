# Project

This endpoint is used to create, update, search or delete project information inside a Terrakube organization. Projects allow you to group workspaces and manage team access at the project level.

{% hint style="warning" %}
To use this endpoint you should have "manageWorkspace" access at team level and a Terrakube organization
{% endhint %}

### Entity fields:

| Path                        | Type   | Description                                      |
| --------------------------- | ------ | ------------------------------------------------ |
| data.type                   | string | Should be "project"                              |
| data.attributes.name        | string | Unique project name for a Terrakube organization |
| data.attributes.description | string | Project description (OPTIONAL)                   |

### Example:

```json
{
    "data": {
        "type": "project",
        "attributes": {
            "name": "My Project",
            "description": "A sample project for grouping workspaces"
        }
    }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/project" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/project" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/project/{projectId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/project/{projectId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/project/{projectId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

### Workspace Assignment

To assign a workspace to a project, use the workspace relationships endpoint. Set `data` to `null` to unassign.

**Assign workspace to project:**

```json
{
    "data": {
        "type": "project",
        "id": "{{projectId}}"
    }
}
```

**Unassign workspace from project:**

```json
{
    "data": null
}
```

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/workspace/{workspaceId}/relationships/project" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
