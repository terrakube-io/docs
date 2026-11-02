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
| plan  | Queue runs; does not grant approve permission. Use `requireApproveToApply` to restrict ungated applies.                                                      |
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

In 2.34.0, `data.attributes.destroyJob` controls destroy permission for Custom access and defaults to `true`. Admin/Write allow destroy; Plan/Read do not. See [Run Restrictions](../../user-guide/workspaces/run-restrictions.md#control-who-can-destroy).

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

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/project/{projectId}/projectAccess" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/project/{projectId}/projectAccess" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/project/{projectId}/projectAccess/{project_accessId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/project/{projectId}/projectAccess/{project_accessId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/project/{projectId}/projectAccess/{project_accessId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
