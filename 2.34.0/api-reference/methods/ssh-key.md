# SSH Key

This endpoint is used to create, update, search or delete ssh keys.

{% hint style="warning" %}
To use this endpoint you should have "manageVcs" access at team level and a Terrakube Organization
{% endhint %}

### Entity fields:

| Path                        | Type   | Description                                       |
| --------------------------- | ------ | ------------------------------------------------- |
| data.type                   | string | Should be "ssh"                                   |
| data.attributes.name        | string | Unique ssh key name for an Terrakube organization |
| data.attributes.description | string | Free text for description                         |
| data.attributes.privateKey  | string | SSH Key content                                   |
| data.attributes.sshType     | string | rsa or ed25519                                    |

### Example:

```
{
  "data": {
    "type": "ssh",
    "attributes": {
      "name": "Sample SSH Key",
      "description": "SSH Key Description",
      "privateKey": "{{sshPrivateKey}}",
      "sshType": "{{sshKeyType}}"
    }
  }
}
```

### Supported Operations

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/ssh" method="get" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/ssh" method="post" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/ssh/{sshId}" method="delete" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}

{% openapi src="../../.gitbook/assets/terrakube-api.json" path="/organization/{organizationId}/ssh/{sshId}" method="patch" %}
[terrakube-api.json](../../.gitbook/assets/terrakube-api.json)
{% endopenapi %}
