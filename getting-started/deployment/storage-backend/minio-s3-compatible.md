# Minio (S3 compatible)

{% hint style="info" %}
This guide will assume that you are using the minikube deployment, but the storage backend can be used in any real kubernetes environment.
{% endhint %}

The first step will be to deploy a minio instance inside minikube in the terrakube namespace

{% hint style="info" %}
MINIO helm deployment [link](https://artifacthub.io/packages/helm/bitnami/minio)
{% endhint %}

Create the file minio-setup.yaml that we can use to create the default user and buckets

```
auth:
  rootUser: "admin"
  rootPassword: "superadmin"
defaultBuckets: "terrakube"
```

```
kubectl install --values minio-setup.yaml miniostorage bitnami/minio -n terrakube
```

Once the minio storage is installed lets get the service name.

```
kubectl get svc -o wide -n terrakube
```

The service name for the minio storage should be "miniostorage"

Once minio is installed with a bucket you will need to get the following:

* access key
* secret key
* bucket name
* endpoint (http://miniostorage:9000)

Now you have all the information we will need to create a terrakube.yaml for our terrakube deployment  with the following content:

```yaml
## Terrakube Storage
storage:
  defaultStorage: false
  minio:
    accessKey: "admin"
    secretKey: "superadmin"
    bucketName: "terrakube"
    endpoint: "http://miniostorage:9000"
```

Now you can install terrakube using the command:

```bash
helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube
```
