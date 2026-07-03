# Collection Item

This endpoint is used to manager collection item for a collection inside the organization

### Entity fields:

| Path                                    | Type    | Description            |
| --------------------------------------- | ------- | ---------------------- |
| data.type                               | string  | Should be "step"       |
| data.attributes.category                | string  | Collection description |
| data.attributes.description             | string  | Collection name        |
| data.attributes.hcl                     | int     | Collection priority    |
| data.attribute.key                      | string  | Collection item name   |
| data.attributes.value                   | string  | Collection item value  |
| <p>data.attributes.s</p><p>ensitive</p> | boolean | Values is sensitive    |

### Example

```json
POST /api/v1/organization/${ORGANIZATION_ID}/collection/"${COLLECTION_ID}/item/

{
    "data": {
        "type": "item",
        "attributes": {
            "category": "ENV",
            "description": "random_description",
            "hcl": false,
            "key": "random_key",
            "sensitive": true
        }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item" method="post" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item/{itemId}" method="get" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item/{itemId}" method="delete" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/v2_7.yaml" path="/organization/{organizationId}/collection/{collectionId}/item/{itemId}" method="patch" %}
[v2_7.yaml](../../.gitbook/assets/v2_7.yaml)
{% endopenapi %}
