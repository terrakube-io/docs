# OpenBao/Vault

### Requirementes

The dynamic provider credential setup in OpenBao/Vault  can be found inside the following link:

[https://github.com/AzBuilder/terrakube/tree/main/dynamic-credential-setup/vault](https://github.com/AzBuilder/terrakube/tree/main/vault)

{% hint style="warning" %}
The code will also create a sample workspace with all the require environment variables that can be used to test the functionality using the CLI driven workflow.
{% endhint %}

Make sure to mount your public and private key to the API container as explained [here](https://docs.terrakube.io/user-guide/workspaces/dynamic-provider-credentials#generate-public-and-private-key)

{% hint style="info" %}
Mare sure the private key is in _**"pkcs8"**_ format
{% endhint %}

Validate the following terrakube api endpoints are working:

* [https://terrakube-api.mydomain.com/.well-known/jwks](https://terrakube-api.mydomain.com/.well-known/jwks)
* [https://terrakube-api.mydomain.com/.well-known/openid-configuration](https://terrakube-api.mydomain.com/.well-known/openid-configuration)

Setup OpenBao/Vault

Running openbao in development mode

```
bao server -dev -dev-root-token-id="dev-only-token" 
```

Use the following openbao policy

```
# Allow tokens to query themselves
path "auth/token/lookup-self" {
  capabilities = ["read"]
}

# Allow tokens to renew themselves
path "auth/token/renew-self" {
  capabilities = ["update"]
}

# Allow tokens to revoke themselves
path "auth/token/revoke-self" {
  capabilities = ["update"]
}

# Configure the actual secrets the token should have access to
path "shared/data/kv/creds" {
  capabilities = ["read"]
}
```

JWT Role

```
{
  "policies": ["tfc-policy"],
  "role_type": "jwt",
  "bound_audiences": ["vault.workload.identity"],
  "bound_claims_type": "glob",
  "user_claim": "terrakube_workspace_name",
  "bound_claims": {
    "sub":"organization:simple:workspace:vault"
  },
  "token_ttl": "20m"
}
```

Adding test values, policy and jwt role to openbao with the following:

```
export VAULT_ADDR='http://localhost:8200'
bao login dev-only-token
bao auth enable userpass
bao write auth/userpass/users/terrakube password=p@ssw0rd
bao auth enable approle
bao write auth/approle/role/terrakube-role secret_id_ttl=10m token_ttl=20m token_max_ttl=30m
bao secrets enable -path shared -version 2 kv
bao kv put -mount shared kv/creds username=terrakube password=p@ssw0rd
bao auth enable jwt
bao write auth/jwt/config oidc_discovery_url="https://terrakube-api.platform.local" bound_issuer="https://terrakube-api.platform.local" 
bao policy write tfc-policy terrakube-policy.hcl
bao write auth/jwt/role/tfc-role @vault-jwt-auth-role.json
```

At the workspace level we need to define the following environment variables:

```
ENABLE_DYNAMIC_CREDENTIALS_VAULT=1
WORKLOAD_IDENTITY_VAULT_AUDIENCE=vault.workload.identity
VAULT_ADDR=http://localhost:8200
WORKLOAD_IDENTITY_VAULT_ROLE=tfc-role
```

Example:

<figure><img src="../../../.gitbook/assets/image (406).png" alt=""><figcaption></figcaption></figure>

Now running cli driven workflow with the following terraform code to test

```
terraform {
  cloud {
    hostname = "terrakube-api.platform.local"
    organization = "simple"
    workspaces {
      name = "vault"
    }
  }
}

provider vault {
  skip_tls_verify = true
  skip_child_token = true
  }

data "vault_kv_secret_v2" "password" {
  mount = "shared"
  name  = "kv/creds"
}
```

It will show the following:

```
user@pop-os:~/git/terrakube/vault$ terraform plan

Running plan in Terraform Cloud. Output will stream here. Pressing Ctrl-C
will stop streaming the logs, but will not stop the plan running remotely.

Preparing the remote plan...

To view this run in a browser, visit:
https://terrakube-api.platform.local/app/simple/vault/runs/run-1

Waiting for the plan to start...

***************************************
Running Terraform PLAN
***************************************
data.vault_kv_secret_v2.password: Reading...
data.vault_kv_secret_v2.password: Read complete after 0s [id=shared/data/kv/creds]

No changes. Your infrastructure matches the configuration.
```
