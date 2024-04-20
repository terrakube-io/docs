# Dynamic Provider Credentials

{% hint style="info" %}
This feature is available from version 2.21.0
{% endhint %}

### Generate Public and Private Key.

To use Azure Dynamic Provider credentials we need to genera a public and private key that will be use to generate a validate the federated tokens, we can use the following commands

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

To use Azure Dynamic Provider credentials the following public endpoints were added and need to be accessible so Azure Entra can validate the federated token.

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
