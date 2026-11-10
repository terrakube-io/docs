# Amazon Cloud Storage

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
