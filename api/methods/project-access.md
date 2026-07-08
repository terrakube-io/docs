# Project Access

This endpoint is used to manage team access within a Terrakube project. Project-level permissions are inherited by all workspaces within the project.

{% hint style="warning" %}
To use this endpoint you should have "manageWorkspace" access at team level or be a project admin
{% endhint %}

### Roles

| Role  | Description                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| admin | Full control: manage workspaces, plan/approve jobs, manage project team access                |
| write | Manage workspaces, plan and apply jobs                                                        |
| plan  | Plan jobs only (cannot approve or apply)                                                      |
| read  | Read-only access (no changes allowed)                                                         |

### Entity fields:

| Path                            | Type    | Description                                                              |
| ------------------------------- | ------- | ------------------------------------------------------------------------ |
| data.type                       | string  | Should be "project\_access"                                              |
| data.attributes.name            | string  | Team name to grant access                                                |
| data.attributes.role            | string  | Role: "admin", "write", "plan", or "read"                               |
| data.attributes.manageWorkspace | boolean | Permission to create, update, and delete workspaces within the project   |
| data.attributes.manageJob       | boolean | Permission to manage jobs within the project                             |
| data.attributes.planJob         | boolean | Permission to plan jobs within the project                               |
| data.attributes.approveJob      | boolean | Permission to approve and apply jobs within the project                  |
| data.attributes.manageState     | boolean | Permission to manage Terraform state within the project                  |

### Example:

```json
{
    "data": {
        "type": "project_access",
        "attributes": {
            "name": "DEVELOPERS",
            "role": "write"
        }
    }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}/projectAccess" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}/projectAccess" method="post" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}/projectAccess/{projectAccessId}" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}/projectAccess/{projectAccessId}" method="patch" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/project/{projectId}/projectAccess/{projectAccessId}" method="delete" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% hint style="info" %}
For a complete list of organization operation please visit the [OpenAPI specification](https://github.com/AzBuilder/terrakube-server/tree/main/openapi-spec)
{% endhint %}
