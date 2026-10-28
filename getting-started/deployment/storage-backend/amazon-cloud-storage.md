# Amazon Cloud Storage

### Using Access Key and Secret

{% hint style="info" %}
This guide will assume that you are using the minikube deployment, but the storage backend can be used in any real kubernetes environment.
{% endhint %}

The first step will be to create one s3 bucket with private access

{% hint style="info" %}
Create S3 bucket tutorial [link](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html)
{% endhint %}

Once the s3 bucket is created you will need to get the following:

* access key
* secret key
* bucket name
* region

Now you have all the information we will need to create a terrakube.yaml for our terrakube deployment  with the following content:

```yaml
## Terrakube Storage
storage:
  defaultStorage: false
  aws:
    accessKey: "rqerqw"
    secretKey: "sadfasfdq"
    bucketName: "qerqw"
    region: "us-east-1"
```

Now you can install terrakube using the command:

```bash
helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube

```

### Using Role Authentication

{% hint style="info" %}
This feature is supported from Terrakube version 2.24.0
{% endhint %}

If need it Terrakube can authenticate without using the access key or secret deploying the storage configuration like the following:

```yaml
## Terrakube Storage
storage:
  defaultStorage: false
  aws:
    bucketName: "my-bucket-name"
    region: "us-east-1"

## Terrakube components properties
api:
  version: "2.24.0"
  env:
  - name: AwsEnableRoleAuth
    value: true

executor:
  version: "2.24.0"
  env:
  - name: AwsIncludeBackendKeys
    value: false
  - name: AwsEnableRoleAuth
    value: true

registry:
  version: "2.24.0"
  env:
  - name: AwsEnableRoleAuth
    value: true
```

### S3-compatible storage (MinIO, Alibaba OSS, Qumulo, ...)

When `AwsEndpoint` points at a custom endpoint, these optional variables on the **api**, **executor** and **registry** adapt Terrakube to the store. The defaults keep existing deployments unchanged. Set them on all three components, since the executor also writes them into the backend it generates for each job.

| Variable | Default | Use it when |
| --- | --- | --- |
| `AwsEndpointRegion` | `auto` | The store needs a real signing region, e.g. `us-east-1`. |
| `AwsPathStyleAccess` | `true` | Set `false` for stores that only accept virtual-hosted style requests (Alibaba OSS answers `SecondLevelDomainForbidden` otherwise). |
| `AwsChunkedEncodingEnabled` | `true` | Set `false` if the store rejects chunked transfer encoding. |
| `AwsChecksumValidationEnabled` | `true` | Set `false` if the store does not support checksum validation. |

### Faster module downloads with presigned URLs

By default the registry streams module zips from storage. Set `AwsS3PresignedRedirectEnabled=true` on the **registry** to redirect clients to a short-lived presigned URL instead, so downloads no longer pass through the registry pod. `AwsS3PresignedUrlExpirySeconds` (default `300`) sets the link lifetime. Azure Blob (`AzureBlobPresignedRedirectEnabled`) and Google Cloud Storage (`GcpPresignedRedirectEnabled`) have the same switches. All are off by default, and the clients must be able to reach the storage endpoint directly.
