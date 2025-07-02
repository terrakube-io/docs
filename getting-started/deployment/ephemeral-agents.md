# Ephemeral Agents

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

<figure><img src="../../.gitbook/assets/image (476).png" alt=""><figcaption></figcaption></figure>

#### Workspace Execution

Now when the job is running internally Terrakube will create a K8S job and will execute each step of the job in a `"ephemeral executor"`

<figure><img src="../../.gitbook/assets/image (477).png" alt=""><figcaption></figcaption></figure>

Internal Kubernetes Job Example:

<figure><img src="../../.gitbook/assets/image (478).png" alt=""><figcaption></figcaption></figure>

Plan Running in a pod:

<figure><img src="../../.gitbook/assets/image (479).png" alt=""><figcaption></figcaption></figure>

Apply Running in a different pod:

<figure><img src="../../.gitbook/assets/image (480).png" alt=""><figcaption></figcaption></figure>

### Node Selector.

{% hint style="warning" %}
Adding node selector configuration is available from version 2.23.0
{% endhint %}

If required you can specify the node selector configuration where the pod will be created using something like the following:

```yaml
api:
  env:
  - name: JAVA_TOOL_OPTIONS
    value: "-Dorg.terrakube.executor.ephemeral.nodeSelector.diskType=ssd -Dorg.terrakube.executor.ephemeral.nodeSelector.nodeType=spot"
```

The above will be the equivalent to use the Kubernetes YAML like:

```yaml
  nodeSelector:
    disktype: ssd
    nodeType: spot
```

### Using Environment Variables for Configuration

{% hint style="warning" %}
This feature is supported from version 2.23.0 or 2.24.0
{% endhint %}

The following environment variables can be used to customize the ephemeral executor adding the following values inside the workspace settings:

* EPHEMERAL\_CONFIG\_NODE\_SELECTOR\_TAGS
  * Example: key1=value1;key2=value2
  * [Reference](https://github.com/AzBuilder/terrakube/pull/1243)
* EPHEMERAL\_CONFIG\_SERVICE\_ACCOUNT
  * Example: myserviceaccount
  * [Reference](https://github.com/AzBuilder/terrakube/pull/1243)
* EPHEMERAL\_CONFIG\_ANNOTATIONS
  * Example: key1=value1;key2=value2
  * [Reference](https://github.com/AzBuilder/terrakube/pull/1243)
* EPHEMERAL\_CONFIG\_TOLERATIONS
  * Example: `key:operator:effect`
  * [`Reference`](https://github.com/AzBuilder/terrakube/pull/1579)
* EPHEMERAL\_CONFIG\_MAP\_NAME
  * [Reference](https://github.com/AzBuilder/terrakube/pull/1505)
* EPHEMERAL\_CONFIG\_MAP\_MOUNT\_PATH
  * [Reference](https://github.com/AzBuilder/terrakube/pull/1505)

More information can be found inside this [code](https://github.com/AzBuilder/terrakube/blob/main/api/src/main/java/org/terrakube/api/plugin/scheduler/job/tcl/executor/ephemeral/EphemeralExecutorService.java)
