# Federated Identity

This endpoint is used to register, update, search or delete OIDC federated identity credentials. Federated identity lets an external OIDC issuer (for example GitHub Actions) call the Terrakube API directly, without a stored Personal Access Token or Team Token.

{% hint style="warning" %}
Federated identity credentials are instance-wide, not scoped to an organization. Only a superuser can create, update, or delete them, even though they are managed from an organization's **Settings > Federated Credentials** tab in the UI.
{% endhint %}

### Entity fields:

| Path                        | Type   | Description                                                                                       |
| ---------------------------- | ------ | --------------------------------------------------------------------------------------------------- |
| data.type                    | string | Should be "federated"                                                                              |
| data.attributes.name         | string | Name for the federated credential. A team with this exact name must exist to grant it permissions. |
| data.attributes.issuerUrl    | string | OIDC token issuer URL, e.g. `https://token.actions.githubusercontent.com`                          |
| data.attributes.audience     | string | Expected `aud` claim, e.g. `api://Terrakube`                                                       |

### Example:

```json
POST /api/v1/federated

{
    "data": {
        "type": "federated",
        "attributes": {
            "name": "github-actions",
            "issuerUrl": "https://token.actions.githubusercontent.com",
            "audience": "api://Terrakube"
        }
    }
}
```

### Supported Operations

{% swagger method="post" path="" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
Register a new federated identity credential.
{% endswagger-description %}

{% swagger-parameter in="body" name="name" type="String" required="true" %}
Federated credential name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="issuerUrl" type="String" required="true" %}
OIDC token issuer URL
{% endswagger-parameter %}

{% swagger-parameter in="body" name="audience" type="String" required="true" %}
Expected token audience
{% endswagger-parameter %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
List all federated identity credentials.
{% endswagger-description %}
{% endswagger %}

{% swagger method="get" path="/{federatedId}" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
Get a single federated identity credential.
{% endswagger-description %}
{% endswagger %}

{% swagger method="patch" path="/{federatedId}" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
Update a federated identity credential.
{% endswagger-description %}
{% endswagger %}

{% swagger method="delete" path="/{federatedId}" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
Delete a federated identity credential. Delete its claims first.
{% endswagger-description %}
{% endswagger %}

### Claim conditions

A federated credential with no claims trusts **any** token from the configured issuer/audience pair. Adding one or more claim conditions requires **every** condition to match the incoming token before it's trusted — use these to scope trust to a specific repository, branch, or environment.

| Path                        | Type   | Description                                                          |
| ---------------------------- | ------ | ------------------------------------------------------------------------ |
| data.type                    | string | Should be "federated\_claim"                                            |
| data.attributes.claimKey     | string | OIDC claim name, e.g. `repository`, `repository_owner`, `ref`, `sub`     |
| data.attributes.claimValue   | string | Required value for that claim. Matches if the claim is a string equal to this value, or a list containing it. |

```json
POST /api/v1/federated/${FEDERATED_ID}/claims

{
    "data": {
        "type": "federated_claim",
        "attributes": {
            "claimKey": "repository",
            "claimValue": "terrakube-io/terrakube"
        }
    }
}
```

{% swagger method="post" path="/{federatedId}/claims" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
Add a claim condition to a federated identity credential.
{% endswagger-description %}
{% endswagger %}

{% swagger method="get" path="/{federatedId}/claims" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
List the claim conditions on a federated identity credential.
{% endswagger-description %}
{% endswagger %}

{% swagger method="delete" path="/{federatedId}/claims/{claimId}" baseUrl="/api/v1/federated" summary="" %}
{% swagger-description %}
Delete a claim condition.
{% endswagger-description %}
{% endswagger %}
