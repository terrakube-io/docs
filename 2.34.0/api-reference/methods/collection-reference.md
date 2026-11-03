# Collection Reference

This endpoint is used to attach (and detach) a [Collection](collection.md) to a workspace. A reference is what makes a variable collection's items actually apply to a given workspace — see [Variable Collections](../../set-up/organizations/variable-collections.md) for the concept and resolution order.

### Entity fields:

| Path                                 | Type   | Description                                     |
| ------------------------------------ | ------ | ----------------------------------------------- |
| data.type                            | string | Should be "reference"                           |
| data.attributes.description          | string | Optional description of the reference           |
| data.relationships.workspace.data.id | string | ID of the workspace to attach the collection to |

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

<mark style="color:green;">`POST`</mark> `/api/v1/organization/{organizationId}/collection/{collectionId}/reference`

Attach a collection to a workspace.

<mark style="color:blue;">`GET`</mark> `/api/v1/organization/{organizationId}/collection/{collectionId}/reference`

List the workspaces a collection is attached to.

<mark style="color:blue;">`GET`</mark> `/api/v1/organization/{organizationId}/collection/{collectionId}/reference/{referenceId}`

Get a single collection reference.

<mark style="color:red;">`DELETE`</mark> `/api/v1/organization/{organizationId}/collection/{collectionId}/reference/{referenceId}`

Detach a collection from a workspace. Does not delete the collection or its variables.
