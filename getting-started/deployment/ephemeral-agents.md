# 🕑 Ephemeral Agents

{% hint style="warning" %}
This feature is supported from version 2.22.0
{% endhint %}

The following will explain how to run the executor component in`"ephemeral"` mode.

These environment variables can be used to customize the API component:

* ExecutorEphemeralNamespace (Default value: "terrakube")
* ExecutorEphemeralImage (Defatul value: "azbuilder/executor:2.22.0" )
* ExecutorEphemeralSecret (Default value: "terrakube-executor-secrets" )

> The above is basically to control where the job will be created and executed and to mount the secrets required by the executor component

Internally the Executor component will use the following to run in `"ephemeral"` :

* EphemeralFlagBatch (Default value: "false")
* EphemeralJobData, this contains all the data that the executor need to run.

### Requirements

To use Ephemeral executors we need to create the following configuration:

#### Service Account Creation

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: terrakube-api-service-account
  namespace: terrakube
```

#### Role Creation

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: terrakube
  name: terrakube-api-role
rules:
- apiGroups: ["batch"]
  resources: ["jobs"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

#### Role Binding Creation

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: terrakube-api-role-binding
  namespace: terrakube
subjects:
- kind: ServiceAccount
  name: terrakube-api-service-account
  namespace: terrakube
roleRef:
  kind: Role
  name: terrakube-api-role
  apiGroup: rbac.authorization.k8s.io
```

#### Helm Chart Configuration

Once the above configuration is created we can deploy the Terrakube API like the following example:

```
## API properties
api:
  image: "azbuilder/api-server"
  version: "2.22.0"
  serviceAccountName: "terrakube-api-service-account"
  env:
  - name: ExecutorEphemeralNamespace
    value: terrakube
  - name: ExecutorEphemeralImage
    value: azbuilder/executor:2.22.0
  - name: ExecutorEphemeralSecret
    value: terrakube-executor-secrets
```

#### Workspace Configuration

Add the environment variable `TERRAKUBE_ENABLE_EPHEMERAL_EXECUTOR=1` like the image below

<figure><img src="../../.gitbook/assets/image (399).png" alt=""><figcaption></figcaption></figure>

#### Workspace Execution

Now when the job is running internally Terrakube will create a K8S job and will execute each step of the job in a `"ephemeral executor"`

<figure><img src="../../.gitbook/assets/image (400).png" alt=""><figcaption></figcaption></figure>

Internal Kubernetes Job Example:

<figure><img src="../../.gitbook/assets/image (401).png" alt=""><figcaption></figcaption></figure>

Plan Running in a pod:

<figure><img src="../../.gitbook/assets/image (402).png" alt=""><figcaption></figcaption></figure>

Apply Running in a different pod:

<figure><img src="../../.gitbook/assets/image (403).png" alt=""><figcaption></figcaption></figure>
