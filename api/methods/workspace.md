# Workspace

This endpoint is used to create, update, search or delete workspace information inside a Terrakube organization.

{% hint style="warning" %}
To use this endpoint you should have "manageWorkspace" access at team level and a Terrakube organization
{% endhint %}

### Entity fields:

| Path                             | Type    | Description                                    |
| -------------------------------- | ------- | ---------------------------------------------- |
| data.type                        | string  | Should be "workspace"                          |
| data.attributes.name             | string  | Unique workspace for an Terrakube organization |
| data.attributes.branch           | boolean | Git branch to be used                          |
| data.attributes.source           | boolean | Git repository to be used                      |
| data.attributes.terraformVersion | boolean | Terraform version to run the workspace         |
| data.relationships.vcs.data.type        | string  | Should be "vcs" (OPTIONAL)                          |
| data.relationships.vcs.data.id          | string  | Should be the VCS Connection Id (OPTIONAL)          |
| data.relationships.project.data.type    | string  | Should be "project" (OPTIONAL)                      |
| data.relationships.project.data.id      | string  | Project ID to assign this workspace to (OPTIONAL)   |

### Example:

```
{
    "data": {
        "type": "workspace",
        "attributes": {
            "branch": "main",
            "name": "Terrakube Workspace",
            "source": "https://github.com/terrakube-io/terraform-sample-repository.git",
            "terraformVersion": "0.15.0"
        },
        "relationships": {
            "vcs": {
                "data": {
                    "type": "vcs",
                    "id": "{{vcsIdGitHub}}"
                }
            }
        }
    }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/workspace/{workspaceId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/workspace" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/workspace/{workspaceId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/workspace/{workspaceId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}
