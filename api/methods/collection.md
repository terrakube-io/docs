# Collection

This endpoint is used to manager collection inside an organization.

### Entity fields:

| Path                        | Type   | Description            |
| --------------------------- | ------ | ---------------------- |
| data.type                   | string | Should be "step"       |
| data.attributes.description | string | Collection description |
| data.attributes.name        | string | Collection name        |
| data.attributes.priority    | int    | Collection priority    |

### Example:

```json
POST /api/v1/organization/${ORGANIZATION_ID}/collection/

{
    "data": {
        "type": "collection",
        "attributes": {
            "description": "Sample Description",
            "name": "Collection1",
            "priority": 10,
        }
}

```

### Supported Operation

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection" method="post" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}" method="delete" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}" method="patch" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}
