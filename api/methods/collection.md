# Collection

This endpoint is used to manage collections inside an organization.

### Entity fields:

| Path                        | Type    | Description            |
| --------------------------- | ------- | ---------------------- |
| data.type                   | string  | Should be "collection" |
| data.attributes.name        | string  | Collection name        |
| data.attributes.description | string  | Collection description |
| data.attributes.priority    | integer | Collection priority    |

### Example:

```json
POST /api/v1/organization/${ORGANIZATION_ID}/collection/

{
    "data": {
        "type": "collection",
        "attributes": {
            "name": "Collection1",
            "description": "Sample Description",
            "priority": 10
        }
    }
}
```

### Supported Operation

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/collection" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/collection" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/collection/{collectionId}" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/collection/{collectionId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/collection/{collectionId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}
