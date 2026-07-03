# Project

This endpoint is used to create, update, search or delete project information inside a Terrakube organization. Projects allow you to group workspaces and manage team access at the project level.

{% hint style="warning" %}
To use this endpoint you should have "manageProject" access at team level and a Terrakube organization
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

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project" method="post" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}" method="patch" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}" method="delete" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
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

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/workspace/{workspaceId}/relationships/project" method="patch" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
