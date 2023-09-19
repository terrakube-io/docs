# Team Tokens

Terrakube allow the creation of team tokens, this will allow the end user to create a token with only the necessary permissions.

{% hint style="warning" %}
Team tokens can be only generated using the API for now.
{% endhint %}

To create a team token we need to first create API Token, after that we can use the generated token to create the team token using the following endpoint:

```
POST {{terrakubeApi}}/access-token/v1/teams
Authorization: Bearer (PERSONAL ACCESS TOKEN)
Request Body:
{
  "days": "1",
  "group": "TERRAKUBE_ADMIN",
  "description": "Sample PAT"
}
```

> A team token can only be generated if you are member of the specified team.

It will generate a token with the following structure.\


<figure><img src="https://user-images.githubusercontent.com/4461895/268794371-cb1648e5-7069-41db-8950-6cce4495935d.png" alt=""><figcaption></figcaption></figure>

To search all the generated team tokens, this method can be used and it will return all the tokens generated for the groups where your users belongs.

```
GET {{terrakubeApi}}/access-token/v1/teams
Authorization: Bearer (PERSONAL ACCESS TOKEN)
Response Body:
[
  {
    "createdDate": "2023-09-18T23:10:02.482+00:00",
    "createdBy": "admin@example.com",
    "updatedDate": "2023-09-18T23:10:02.482+00:00",
    "updatedBy": "admin@example.com",
    "id": "a00d2ee2-81d3-4e2b-ae5f-6a2edd43a0bb",
    "days": 1,
    "group": "TERRAKUBE_ADMIN",
    "description": "Sample PAT"
  }
]
```

To validate the current teams for your user before generate a team token this can be used:

```
GET {{terrakubeApi}}/access-token/v1/teams/current-teams
Authorization: Bearer (PERSONAL ACCESS TOKEN)
Response Body:
{
  "groups": [
    "TERRAKUBE_ADMIN",
    "TERRAKUBE_DEVELOPERS"
  ]
}
```



{% hint style="info" %}
This feature is supported from Terrakube 2.16.0
{% endhint %}
