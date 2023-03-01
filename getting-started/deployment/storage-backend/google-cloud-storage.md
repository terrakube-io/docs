# Google Cloud Storage

{% hint style="info" %}
This guide will assume that you are using the minikube deployment, but the storage backend can be used in any real kubernetes environment.
{% endhint %}

The first step will be to create one google storage bucket with private access

{% hint style="info" %}
Create storage bucket tutorial [link](https://cloud.google.com/storage/docs/creating-buckets)
{% endhint %}

Once the google storage bucket is created you will need to get the following:

* project id
* bucket name
* JSON GCP credentials file with access to the storage bucket

Now you have all the information we will need to create a terrakube.yaml for our terrakube deployment  with the following content:

```yaml
## Terrakube Storage
storage:
  defaultStorage: false
  gcp:
    projectId: "sample project"
    bucketName: "sampledata"
    credentials: |
      {
        "type": "service_account",
        "project_id": "XXXXXXX",
        ......
      }
```

Now you can install terrakube using the command:

```bash
helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube
```
