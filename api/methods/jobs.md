# Jobs

This endpoint is used to create, update, search or delete jobs for a particular Terrakube workspace.

{% hint style="warning" %}
To use this endpoint you need to create a Terrakube workspace.
{% endhint %}

### Entity fields:

| Path                                   | Type   | Description                                       |
| -------------------------------------- | ------ | ------------------------------------------------- |
| data.type                              | string | Should be "job"                                   |
| data.attributes.overrideBranch         | string | Branch that will be use to execute the job        |
| data.attributes.templateReference      | string | Terrakube template id to use when running the job |
| data.relationships.workspace.data.type | string | Should be "workspace"                             |
| data.relationships.workspace.data.Id   | string | Should be the VCS Workspace Id (OPTIONAL)         |

{% hint style="info" %}
To better understand Terrakube Configuration Language job templates please refer to the following [GitHub repository.](https://github.com/terrakube-io/terrakube-extensions)
{% endhint %}

### Example:

```
{
  "data": {
    "type": "job",
    "attributes": {
      "overrideBranch": "main"
      "templateReference": "XXXXXXXXX"
    },
    "relationships":{
        "workspace":{
            "data":{
                "type": "workspace",
                "id": "{{workspaceId}}"
            }
        }
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/job" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/job/{jobId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/job/{jobId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/job/{jobId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% hint style="info" %}
The complete OpenAPI specification is available as [terrakube-api.json](../../.gitbook/assets/terrakube-api.json), and in the UI under **Help & Support → API Docs**.
{% endhint %}

### Run checks in 2.34.0

Creating a job requires plan permission. Workspace branch restrictions and approval rules apply to REST/GraphQL too, and destroy runs have a separate `destroyJob` permission. An explicit `commitId` is refused for apply-capable templates when branch restriction is enabled. See [Run Restrictions](../../user-guide/workspaces/run-restrictions.md) for the checks and defaults, and [API-driven Workflow](../../user-guide/workspaces/api-driven-workflow.md) for run-only variables.
