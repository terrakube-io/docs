# AWS Dynamic Provider Credentials

### Requirements

The dynamic provider credential setup in AWS  can be done with the Terrraform code available in the following link:

[https://github.com/AzBuilder/terrakube/tree/main/dynamic-credential-setup/aws](https://github.com/AzBuilder/terrakube/tree/main/dynamic-credential-setup/aws)

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

Set terraform variables using: _**"variables.auto.tfvars"**_

```
terrakube_token = "TERRAKUBE_PERSONAL_ACCESS_TOKEN"
terrakube_api_hostname = "TERRAKUBE-API.MYCLUSTER.COM"
terrakube_federated_credentials_audience="aws.workload.identity"
terrakube_organization_name="simple"
terrakube_workspace_name = "dynamic-workspace-aws"
aws_region = "us-east-1"
```

{% hint style="info" %}
To generate the API token check [here](https://docs.terrakube.io/user-guide/organizations/api-tokens)
{% endhint %}

Run Terraform apply to create all the federated credential setup in AWS  and a sample workspace in terrakube for testing

To test the following terraform code can be used:

```
terraform {

  cloud {
    organization = "terrakube_organization_name"
    hostname = "terrakube-api.mydomain.com"

    workspaces {
      name = "terrakube_workspace_name"
    }
  }
}

provider "aws" {


}

resource "aws_s3_bucket" "example" {
  bucket = "my-tf-superbucket-awerqerq"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}
```

<figure><img src="../../../.gitbook/assets/image (469).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (468).png" alt=""><figcaption></figcaption></figure>

### Session tags (ABAC)

The AWS web identity token Terrakube issues can also carry AWS session tags, so IAM roles can be scoped with `aws:PrincipalTag` (attribute-based access control) instead of creating one role per workspace.

Session tags are opt-in: set the workspace environment variable `ENABLE_AWS_SESSION_TAGS` to `true` to enable them. This requires `sts:TagSession` in the role's trust policy — without it, `AssumeRoleWithWebIdentity` fails. Workspaces that don't set this variable are unaffected.

Three tags are sent as transitive principal tags:

| Tag                    | Value                                                |
| ------------------------ | --------------------------------------------------------- |
| `terrakube:org`           | Organization name                                          |
| `terrakube:workspace`     | Workspace name                                              |
| `terrakube:project`       | Project name (only set when the workspace belongs to a project) |

The role trust policy needs two additions: allow `sts:TagSession` alongside `sts:AssumeRoleWithWebIdentity`, and restrict accepted tag keys with a `ForAllValues:StringEquals` condition on `aws:TagKeys` so Terrakube can't set any tag key beyond the ones you expect:

```json
"Action": [
  "sts:AssumeRoleWithWebIdentity",
  "sts:TagSession"
],
"Condition": {
  "StringEquals": {
    "terrakube-api.mydomain.com:aud": "aws.workload.identity"
  },
  "StringLike": {
    "terrakube-api.mydomain.com:sub": "organization:my-org:workspace:*"
  },
  "ForAllValues:StringEquals": {
    "aws:TagKeys": [
      "terrakube:org",
      "terrakube:workspace",
      "terrakube:project"
    ]
  }
}
```

This lets a single role serve every workspace in an organization while still scoping access per workspace, for example restricting an S3 prefix to the calling workspace's name:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::my-terrakube-bucket/${aws:PrincipalTag/terrakube:workspace}/*"
    }
  ]
}
```

You can further restrict accepted tag *values* (not just keys) with `aws:RequestTag/<key>` in the trust policy, to pin a role to a specific organization or workspace.
