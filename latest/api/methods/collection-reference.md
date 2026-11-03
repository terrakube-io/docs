# Collection Reference

This endpoint is used to attach (and detach) a [Collection](collection.md) to a workspace. A reference is what makes a variable collection's items actually apply to a given workspace — see [Variable Collections](../../user-guide/organizations/variable-collections.md) for the concept and resolution order.

### Entity fields:

| Path                                | Type   | Description                                  |
| ------------------------------------- | ------ | ----------------------------------------------- |
| data.type                             | string | Should be "reference"                          |
| data.attributes.description           | string | Optional description of the reference          |
| data.relationships.workspace.data.id  | string | ID of the workspace to attach the collection to |

### Example:

```json
POST /api/v1/organization/${ORGANIZATION_ID}/collection/${COLLECTION_ID}/reference

{
    "data": {
        "type": "reference",
        "attributes": {
            "description": "Applies shared AWS provider config"
        },
        "relationships": {
            "workspace": {
                "data": {
                    "type": "workspace",
                    "id": "${WORKSPACE_ID}"
                }
            }
        }
    }
}
```

### Supported Operations

{% swagger method="post" path="" baseUrl="/api/v1/organization/{organizationId}/collection/{collectionId}/reference" summary="" %}
{% swagger-description %}
Attach a collection to a workspace.
{% endswagger-description %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/api/v1/organization/{organizationId}/collection/{collectionId}/reference" summary="" %}
{% swagger-description %}
List the workspaces a collection is attached to.
{% endswagger-description %}
{% endswagger %}

{% swagger method="get" path="/{referenceId}" baseUrl="/api/v1/organization/{organizationId}/collection/{collectionId}/reference" summary="" %}
{% swagger-description %}
Get a single collection reference.
{% endswagger-description %}
{% endswagger %}

{% swagger method="delete" path="/{referenceId}" baseUrl="/api/v1/organization/{organizationId}/collection/{collectionId}/reference" summary="" %}
{% swagger-description %}
Detach a collection from a workspace. Does not delete the collection or its variables.
{% endswagger-description %}
{% endswagger %}
