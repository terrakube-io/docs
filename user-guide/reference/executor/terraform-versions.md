# Terraform Versions

Terrakube is using the following java library to handle all the different terraform versions:

[https://github.com/AzBuilder/terraform-spring-boot](https://github.com/AzBuilder/terraform-spring-boot)

This library can download different terraform versions and it is used to execute all the terraform commands, each terraform command is executed a new process instance at the OS level

The terrafom files are downloaded to the foloder

/home/cnb/.terraform-spring-boot/download&#x20;

{% hint style="info" %}
The library download the tar.gz files using the terraform index

https://releases.hashicorp.com/terraform/index.json

This can be changed using the environment variable CustomTerraformReleasesUrl

For more information check [https://docs.terrakube.org/getting-started/deployment/custom-terraform-cli-builds](https://docs.terrakube.org/getting-started/deployment/custom-terraform-cli-builds)
{% endhint %}

Terraform binary is extracted to:

```
/home/cnb/.terraform-spring-boot/terraform/{{VERSION}}
```

The correct terraform PATH is add the the executor execution when running a job

