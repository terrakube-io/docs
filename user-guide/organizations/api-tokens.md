# API Tokens

At the moment Terrakube supports only user level API tokens.&#x20;

API tokens are displayed only once when they are created, and are obfuscated thereafter. If the token is lost, it must be regenerated.

### User API Tokens <a href="#user-api-tokens" id="user-api-tokens"></a>

API tokens may belong directly to a user. User tokens inherit permissions from the user they are associated with.

User API tokens can be generated inside the **User Settings**.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

Click the **Generate and API token** button

<figure><img src="../../.gitbook/assets/image (1) (6).png" alt=""><figcaption></figcaption></figure>

Add some small description to the token and the duration

<figure><img src="../../.gitbook/assets/image (3) (1) (2) (2).png" alt=""><figcaption></figcaption></figure>

The new token will be showed and you can copy it to star calling the Terrakube API using Postman or some other tool.

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

#### Code example:

```
curl --location --request POST 'https://{TerrakubeAPI}/api/v1/organization' \
--header 'Content-Type: application/vnd.api+json' \
--header 'Authorization: Bearer XXXXXXX' \
--data-raw '{
  "data": {
    "type": "organization",
    "attributes": {
      "name": "hashicorp",
      "description": "hashicorp organization"
    }
  }
}'
```

{% hint style="info" %}
Replace XXXXXXX with the Personal Access Token value
{% endhint %}
