# Personal Access Token (PAT)

Terrakube allows to create tokens that you can use to call the API, this tokens can be generated inside the user settings.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Click "Genarate and API token"&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Add some small description to the token and the duration

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

The new token will be showed and you can copy it to star calling the Terrakube API using Postman or some other tool.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

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
