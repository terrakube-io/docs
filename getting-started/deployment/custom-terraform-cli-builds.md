# ⚙ Custom Terraform CLI Builds

Terrakube will support terraform cli custom builds or custom cli mirrors, the only requirement is to expose an endpoint with the following structure:

{% hint style="info" %}
Support is available from version 2.12.0
{% endhint %}

Example Endpoint:&#x20;

* https://eov1ys4sxa1bfy9.m.pipedream.net/

```json
{
   "name":"terraform",
   "versions":{
      "1.3.9":{
         "builds":[
            {
               "arch":"amd64",
               "filename":"terraform_1.3.9_linux_amd64.zip",
               "name":"terraform",
               "os":"linux",
               "url":"https://releases.hashicorp.com/terraform/1.3.9/terraform_1.3.9_linux_amd64.zip",
               "version":"1.3.9"
            }
         ],
         "name":"terraform",
         "shasums":"terraform_1.3.9_SHA256SUMS",
         "shasums_signature":"terraform_1.3.9_SHA256SUMS.sig",
         "shasums_signatures":[
            "terraform_1.3.9_SHA256SUMS.72D7468F.sig",
            "terraform_1.3.9_SHA256SUMS.sig"
         ],
         "version":"1.3.9"
      }
   }
}
```

{% hint style="success" %}
This is usefull when you have some network restrictions that does not allow to get the information from [https://releases.hashicorp.com/terraform/index.json](https://releases.hashicorp.com/terraform/index.json) in your private kuberentes cluster.
{% endhint %}

To support custom terrafom cli releases when using the helm chart us the following:

```
api:
  version: "2.12.0"
  terraformReleasesUrl: "https://eov1ys4sxa1bfy9.m.pipedream.net/"

executor:
  version: "2.12.0"
```
