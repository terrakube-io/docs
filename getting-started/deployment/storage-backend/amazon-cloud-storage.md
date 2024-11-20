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
    accessKey: "12345"
    secretKey: "12345"
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

{% hint style="info" %}
Using "12345" for the access key and secret key is required because the helm chart require those values internally.
{% endhint %}
