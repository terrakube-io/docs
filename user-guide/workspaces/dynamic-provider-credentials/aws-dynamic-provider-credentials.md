# AWS Dynamic Provider Credentials

To use AWS Dynamic Provider Credentials to authenticate to AWS without storing any sensitive credential in a workspace please use the following:

To do the complete setup with Terraform use the following and change the variables depending of your environment:

Input variables:

```
terrakube_api_hostname = "TERRAKUBE-API.MYCLUSTER.COM"
terrakube_federated_credentials_audience="aws.workload.identity"
terrakube_organization="simple"
terrakube_workspace = "simple-cli"
```

Terraform Code

{% hint style="warning" %}
The below aws\_iam\_policy can be customize in this case we will use it just to deploy S3 as an example
{% endhint %}

```
provider "aws" {

}

variable "terrakube_api_hostname" {
  type        = string
  default     = "terrakube-api.minikube.net"
  description = "The terrakube API hostname example: terrakube-api.minikube.net"
}

variable "terrakube_federated_credentials_audience" {
  type        = string
  default     = "aws.workload.identity"
  description = "Audience for the federated credentials"
}

variable "terrakube_organization" {
  type        = string
  default     = "simple"
  description = "Audience for the federated credentials"
}

variable "terrakube_workspace" {
  type        = string
  default     = "simple"
  description = "Audience for the federated credentials"
}

data "tls_certificate" "terrakube_certificate" {
  url = "https://${var.terrakube_api_hostname}"
}

resource "aws_iam_openid_connect_provider" "terrakube_provider" {
  url             = data.tls_certificate.terrakube_certificate.url
  client_id_list  = [var.terrakube_federated_credentials_audience]
  thumbprint_list = [data.tls_certificate.terrakube_certificate.certificates[0].sha1_fingerprint]
}

resource "aws_iam_role" "terrakube_role" {
  name = "terrakube-role"

  assume_role_policy = <<EOF
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Effect": "Allow",
     "Principal": {
       "Federated": "${aws_iam_openid_connect_provider.terrakube_provider.arn}"
     },
     "Action": "sts:AssumeRoleWithWebIdentity",
     "Condition": {
        "StringEquals": {
        "${var.terrakube_api_hostname}:aud": "${var.terrakube_federated_credentials_audience}",
        "${var.terrakube_api_hostname}:sub": "organization:${var.terrakube_organization}:workspace:${var.terrakube_workspace}"
        }
     }
   }
 ]
}
EOF
}

resource "aws_iam_policy" "terrakube_policy" {
  name        = "terrakube-policy"
  description = "terrakube policy"

  policy = <<EOF
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Effect": "Allow",
     "Action": [
       "s3:*"
     ],
     "Resource": "*"
   }
 ]
}
EOF
}

resource "aws_iam_role_policy_attachment" "terrakube_policy_attachment" {
  role       = aws_iam_role.terrakube_role.name
  policy_arn = aws_iam_policy.terrakube_policy.arn
}

output "role_arn" {
  value = aws_iam_role.terrakube_role.arn
}
```

The identity provider should look like the following after running the above code:

<figure><img src="../../../.gitbook/assets/image (386).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (387).png" alt=""><figcaption></figcaption></figure>

The IAM role will look like the following

<figure><img src="../../../.gitbook/assets/image (388).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (389).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (390).png" alt=""><figcaption></figcaption></figure>

Now that the AWS setup is completed we only need to update our workspace in Terrakube with the following environment variables:

* ENABLE\_DYNAMIC\_CREDENTIALS\_AWS=true
* WORKLOAD\_IDENTITY\_AUDIENCE\_AWS=aws.workload.identity
* WORKLOAD\_IDENTITY\_ROLE\_AWS=XXXXXXXXX
* AWS\_REGION=XXXXXXXXX

And finally when we execute the job we will see that Terrakube automatically authenticate to AWS:

Terraform code for the workspace:

```
provider "aws" {

}

terraform {

  cloud {
    organization = "simple"
    hostname = "TERRAKUBE.API.COM"

    workspaces {
      name = "simple-cli"
    }
  }
}

resource "aws_s3_bucket" "example" {
  bucket = "my-tf-test-asdfafasfqerqrqw"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}
```

<figure><img src="../../../.gitbook/assets/image (392).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (391).png" alt=""><figcaption></figcaption></figure>
