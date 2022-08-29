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

{% swagger src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/ssh" method="get" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/ssh" method="post" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/ssh/{sshId}" method="delete" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endswagger %}

{% swagger src="../../.gitbook/assets/v2_6.yaml" path="/organization/{organizationId}/ssh/{sshId}" method="patch" %}
[v2_6.yaml](../../.gitbook/assets/v2_6.yaml)
{% endswagger %}
