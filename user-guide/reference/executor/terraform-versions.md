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

For more information check [https://docs.terrakube.io/getting-started/deployment/custom-terraform-cli-builds](https://docs.terrakube.io/getting-started/deployment/custom-terraform-cli-builds)
{% endhint %}

### Binary caching

Before downloading a Terraform or OpenTofu binary, an executor first checks whether it's already cached in the workspace's storage backend (Amazon S3, Azure Storage, GCP Cloud Storage, or local disk — whichever [storage backend](../../../getting-started/deployment/storage-backend/ "mention") the instance is configured with) and restores it from there instead. If it's not cached anywhere, the executor downloads it from the release index as before and uploads a copy to storage for the next executor pod that needs that version — so a freshly scaled-up or replaced pod doesn't have to re-download the same binary from HashiCorp/GitHub that every other pod already has.

{% hint style="info" %}
If the release index itself (`releases.hashicorp.com` or an OpenTofu/custom equivalent) is temporarily unreachable, Terrakube falls back to the last successfully fetched copy of that index instead of failing outright, so brief upstream outages don't block new jobs from resolving a Terraform/OpenTofu version.
{% endhint %}

Terraform binary is extracted to:

```
/home/cnb/.terraform-spring-boot/terraform/{{VERSION}}
```

The correct terraform PATH is add the the executor execution when running a job

### OpenTofu

A workspace's IaC tool (`terraform` or `tofu`) is chosen per-workspace (**Settings > General > IaC Type**, or `--iac-type` on `terrakube workspace create`/`update`). When a workspace is set to `tofu`, the executor downloads and runs OpenTofu binaries instead of Terraform, following the same download/extract flow described above.

{% hint style="info" %}
OpenTofu releases are indexed separately from Terraform, defaulting to the OpenTofu GitHub releases API instead of `releases.hashicorp.com`. This index can be overridden with the `CustomTofuReleasesUrl` environment variable, the OpenTofu equivalent of `CustomTerraformReleasesUrl` — see [Custom Terraform CLI Builds](../../../getting-started/deployment/custom-terraform-cli-builds.md).
{% endhint %}

