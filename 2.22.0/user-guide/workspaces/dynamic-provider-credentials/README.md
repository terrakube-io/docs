# Dynamic Provider Credentials

{% hint style="info" %}
This feature is available from version 2.21.0
{% endhint %}

### Generate Public and Private Key.

To use Dynamic Provider credentials we need to genera a public and private key that will be use to generate a validate the federated tokens, we can use the following commands

```
openssl genrsa -out private_temp.pem 2048
openssl rsa -in private_temp.pem -outform PEM -pubout -out public.pem
```

You need to make sure the private key starts with "-----BEGIN PRIVATE KEY-----" if not the following command can be used to transform the private key to the correct format

```
openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in private_temp.pem -out private.pem
```

The public and private key need to be mounted inside the container and the path should be specify in the following environment variables

* DynamicCredentialPublicKeyPath
* DynamicCredentialPrivateKeyPath

### Public Endpoints Requirements

To use Dynamic Provider credentials the following public endpoints were added. This endpoint needs to be accessible for your different cloud providers.

```
GET https://TERRAKUBE.MYSUPERDOMAIN.COM/.well-known/openid-configuration
{
  "issuer": "https://TERRAKUBE.MYSUPERDOMAIN.COM",
  "jwks_uri": "https://TERRAKUBE.MYSUPERDOMAIN.COM/.well-known/jwks",
  "response_types_supported": [
    "id_token"
  ],
  "claims_supported": [
    "sub",
    "aud",
    "exp",
    "iat",
    "iss",
    "jti",
    "terrakube_workspace_id",
    "terrakube_organization_id",
    "terrakube_job_id"
  ],
  "id_token_signing_alg_values_supported": [
    "RS256"
  ],
  "scopes_supported": [
    "openid"
  ],
  "subject_types_supported": [
    "public"
  ]
}
```

```
GET https://TERRAKUBE.MYSUPERDOMAIN.COM/.well-known/jwks
{
  "keys": [
    {
      "kty": "RSA",
      "use": "sig",
      "n": "ALEzGE4Rn2WhxOhIuXAzq7e-WvRLCJfoqrHMUXtpt6gefNmWGo9trbea84KyeKdvzE9wBwWxnz_U5d_utmLLztVA2FLdDfnndh7pF4Fp7hB-lhaT1hV2EsiFsc9oefCYmkzXmHylfNQOuqNlRA_2Xu5pHovrF79WW01hWSjhGTkpj6pxFG4t7Tl54SWnJ83CvGDAKuoO9c1M1iTKikB3ENMK8WfU-wZJ4oLTAfhSydqZxZuGRhiwPGsEQOpRynyHJ54XWZHmFdsWs_eGRsfs1iTPbiQSBZbaEwz36HF4QdqFzzLGd67sTtZku_YEsUbJW8cbK6nOFEdR0BSTtSV-lPk=",
      "e": "AQAB",
      "kid": "03446895-220d-47e1-9564-4eeaa3691b42",
      "alg": "RS256"
    }
  ]
}
```

### Terrakube Environment Variables:

The following environment variables can be used to customize the dynamic credentials configuration:

* DynamicCredentialId = This will be the kid in the JWKS endpoint (Default value: 03446895-220d-47e1-9564-4eeaa3691b42)
* DynamicCredentialTtl= The TTL for the federated token generated internally in Terrakube (Defafult: 30)
* DynamicCredentialPublicKeyPath= The path to the public key to validate the federated tokens
* DynamicCredentialPrivateKeyPath=The path to the private key to generate the federated tokens

### Token structure

Terrakube will generate a JWT token internally, this token will be used to authenticate to your cloud provider.

The token structure looks like the following for Azure

```
JWT HEADER
{
  "kid": "12345",
  "alg": "RS256"
}
JWT BODY
{
  "sub": "organization:TERRAKUBE_ORG_NAME:workspace:TERRAKUBE_WORKSPACE_NAME",
  "aud": "api://AzureADTokenExchange",
  "jti": "12345678",
  "terrakube_workspace_id": "1",
  "terrakube_organization_id": "2",
  "terrakube_job_id": "3",
  "iat": 1713397293,
  "iss": "https://terrakube-api.example.com",
  "exp": 1713397353
}
SIGNATURE
```

The token structure looks like the following for GCP

```
JWT HEADER
{
  "kid": "03446895-220d-47e1-9564-4eeaa3691b42",
  "alg": "RS256"
}
JWT BODY
{
  "sub": "organization:TERRAKUBE_ORG_NAME:workspace:TERRAKUBE_WORKSPACE_NAME",
  "aud": "https://iam.googleapis.com/projects/xxxxx/locations/global/workloadIdentityPools/xxxxx/providers/xxxxx",
  "jti": "d4432299-5dad-4b1e-9756-544639e84cec",
  "terrakube_workspace_id": "d9b58bd3-f3fc-4056-a026-1163297e80a8",
  "terrakube_organization_id": "8abe206b-29a8-4ed8-8a3b-30237e295659",
  "terrakube_job_id": "1",
  "iat": 1713915600,
  "iss": "https://terrakube-api.example.com",
  "exp": 1713917400
}
SIGNATURE
```

The token structure looks like the following for AWS

```
JWT HEADER
{
  "kid": "03446895-220d-47e1-9564-4eeaa3691b42",
  "alg": "RS256"
}
JWT BODY
{
  "sub": "organization:TERRAKUBE_ORG_NAME:workspace:TERRAKUBE_WORKSPACE_NAME",
  "aud": "aws.workload.identity",
  "jti": "d4432299-5dad-4b1e-9756-544639e84cec",
  "terrakube_workspace_id": "d9b58bd3-f3fc-4056-a026-1163297e80a8",
  "terrakube_organization_id": "8abe206b-29a8-4ed8-8a3b-30237e295659",
  "terrakube_job_id": "1",
  "iat": 1713915600,
  "iss": "https://terrakube-api.example.com",
  "exp": 1713917400
}
SIGNATURE
```
