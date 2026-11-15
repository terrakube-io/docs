# Google Cloud Storage

### Using a Service Account Key

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

### Using Application Default Credentials (GKE Workload Identity)

{% hint style="info" %}
This feature is supported from Terrakube version 2.35.0 and Helm chart version 4.7.19
{% endhint %}

When `credentials` is left empty, the api, executor and registry authenticate to Google Cloud Storage with [Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials). On GKE this lets Terrakube use [Workload Identity Federation for GKE](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity) instead of a long-lived service account key stored in a Kubernetes secret. When the executor generates a `gcs` backend for a job (with the HTTP state backend disabled), it leaves out the key as well, so Terraform and OpenTofu use the same credentials.

The steps below bind the Kubernetes service accounts of the api, executor and registry to one Google service account that can use the bucket. They assume Workload Identity is enabled on the cluster, Terrakube is installed in the `terrakube` namespace, and the following names:

* Google Cloud project: `my-project`
* Bucket: `terrakube-bucket`
* Google service account: `terrakube-storage@my-project.iam.gserviceaccount.com`
* Kubernetes service accounts: `terrakube-api`, `terrakube-executor` and `terrakube-registry`

Create the Google service account and allow it to read and write the objects in the bucket:

```bash
gcloud iam service-accounts create terrakube-storage --project my-project

gcloud storage buckets add-iam-policy-binding gs://terrakube-bucket \
  --member "serviceAccount:terrakube-storage@my-project.iam.gserviceaccount.com" \
  --role "roles/storage.objectAdmin"
```

Allow each Kubernetes service account to act as the Google service account:

```bash
for ksa in terrakube-api terrakube-executor terrakube-registry; do
  gcloud iam service-accounts add-iam-policy-binding terrakube-storage@my-project.iam.gserviceaccount.com \
    --role "roles/iam.workloadIdentityUser" \
    --member "serviceAccount:my-project.svc.id.goog[terrakube/${ksa}]"
done
```

Create the Kubernetes service accounts and annotate them with the Google service account. If the Helm chart creates the api service account (`api.rbac.create` or `api.ephemeralExecution.enabled` is `true`), leave `terrakube-api` out of this loop and set the annotation through `api.serviceAccountAnnotations` instead:

```bash
for ksa in terrakube-api terrakube-executor terrakube-registry; do
  kubectl create serviceaccount ${ksa} -n terrakube
  kubectl annotate serviceaccount ${ksa} -n terrakube \
    iam.gke.io/gcp-service-account=terrakube-storage@my-project.iam.gserviceaccount.com
done
```

Then deploy Terrakube without `credentials`:

```yaml
## Terrakube Storage
storage:
  defaultStorage: false
  gcp:
    projectId: "my-project"
    bucketName: "terrakube-bucket"

## Terrakube components properties
api:
  serviceAccountName: "terrakube-api"
  ## Only needed when the chart creates the api service account
  # serviceAccountAnnotations:
  #   iam.gke.io/gcp-service-account: "terrakube-storage@my-project.iam.gserviceaccount.com"

executor:
  serviceAccountName: "terrakube-executor"

registry:
  serviceAccountName: "terrakube-registry"
```

If no credentials are configured and Application Default Credentials are not available, the component fails at startup with a message saying so.

#### Ephemeral executors

[Ephemeral executor](../ephemeral-agents.md) jobs need a service account with the same access. Set the `EPHEMERAL_CONFIG_SERVICE_ACCOUNT` workspace variable to a Kubernetes service account bound to the Google service account, for example `terrakube-executor`, in the namespace where the jobs run.

#### Registry presigned redirects

When `GcpPresignedRedirectEnabled` is `true`, the registry sends Terraform a V4-signed URL for each `module.zip`. Without a key, the URL is signed through the IAM `signBlob` API, so the Google service account needs the `iam.serviceAccounts.signBlob` permission on itself:

```bash
gcloud iam service-accounts add-iam-policy-binding terrakube-storage@my-project.iam.gserviceaccount.com \
  --role "roles/iam.serviceAccountTokenCreator" \
  --member "serviceAccount:terrakube-storage@my-project.iam.gserviceaccount.com"
```

Signing needs a Google service account. If the registry uses Workload Identity principals directly, without a Google service account, leave `GcpPresignedRedirectEnabled` off so the registry serves the module bytes itself. Credentials that can never sign a URL are refused when the registry starts.

#### Security considerations

Terraform code runs in the executor pod, so a `google` provider block without explicit credentials authenticates as the executor's identity through the GKE metadata server. With a service account key this was already possible by reading the key file from the working directory. Keep the Google service account limited to the Terrakube bucket, and use [GCP Dynamic Provider Credentials](../../../user-guide/workspaces/dynamic-provider-credentials/gcp-dynamic-provider-credentials.md) for the resources that a workspace manages.
