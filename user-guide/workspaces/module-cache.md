# Module Cache

The executor runs every job in a fresh clone of the workspace repository and deletes it afterwards. Terraform and OpenTofu keep the modules they download and the providers they install inside the `.terraform` directory of that clone, so **each job downloads every module and provider again**. The [Provider Cache](provider-cache.md) covers the providers; this page explains how to also keep the modules (and providers) between jobs of the same workspace.

Terraform and OpenTofu support relocating the `.terraform` directory with the `TF_DATA_DIR` environment variable. When it points at a directory that already contains the modules and providers of a previous run, `init` reuses them and downloads nothing:

```
Initializing modules...
Initializing provider plugins...
- Reusing previous version of keycloak/keycloak from the dependency lock file
- Using previously-installed keycloak/keycloak v5.8.0
Terraform has been successfully initialized!
```

There are three ways to set it up.

## Helm chart

Chart version 4.8.0 and later has an `executor.cache` block that mounts a cache volume and sets every variable described below in one place:

```yaml
executor:
  cache:
    enabled: true
    path: /home/cnb/.terraform.d   # mount point of the cache volume
    sizeLimit: 2Gi                 # emptyDir size; or use existingClaim for a PVC
    existingClaim: ""              # RWX PVC when replicaCount > 1
    providers: true                # TF_PLUGIN_CACHE_DIR=<path>/plugin-cache
    ignoreLockFile: false          # TF_PLUGIN_CACHE_MAY_BREAK_DEPENDENCY_LOCK_FILE=1
    modules: true                  # TerraformDataDirCacheRoot=<path>/data (see below)
```

## Executor setting (automatic, per workspace)

Set the executor environment variable `TerraformDataDirCacheRoot` to a directory on a mounted volume, for example `/home/cnb/.terraform.d/data`. For every job the executor then creates `<root>/<organizationId>/<workspaceId>` and runs terraform/tofu with `TF_DATA_DIR` pointing at it, so the next job of the same workspace finds its modules and providers already in place. A `TF_DATA_DIR` defined as a workspace environment variable always takes precedence. This setting is ignored by executors that predate it.

## Workspace environment variable (any version)

Add an environment variable to the workspace:

```
TF_DATA_DIR=/home/cnb/.terraform.d/data/<workspace-name>
```

The directory must be on a volume mounted in the executor (the same `cache-volume` used by the provider cache works) and must be unique per workspace: never share one data directory between workspaces, because it also stores the backend configuration and the module manifest of that configuration.

## Things to know

* `terraform init` is executed without `-upgrade`, so a cached module or provider is kept as long as it satisfies the version constraint in the configuration. Pin module and provider versions; with a loose constraint such as `~> 1.0` a newer release is only picked up after the cache directory is deleted.
* Providers are reused from the data directory when the workspace commits its `.terraform.lock.hcl`. Without a lock file terraform re-verifies them against the registry on every run; use the provider cache with `TF_PLUGIN_CACHE_MAY_BREAK_DEPENDENCY_LOCK_FILE=1`, or commit the lock file (`terraform providers lock -platform=linux_amd64`).
* Terrakube runs one job at a time per workspace, so no two jobs write to the same data directory. With several executor replicas sharing one volume the claim must be `ReadWriteMany`.
* Space: one copy of each module call plus one copy of each provider version per workspace. An emptyDir is emptied when the pod restarts; use a PersistentVolumeClaim to keep the cache across restarts.
* To reset a workspace's cache simply delete its directory under the cache root.
* Terraform does not create `TF_PLUGIN_CACHE_DIR` itself ("the directory must already exist"), and a freshly mounted volume is empty, so `init` reports `The specified plugin cache dir ... cannot be opened`. The chart's `executor.cache` block creates the sub-directories with an init container; if you mount the volume by hand, add an init container (or a pre-init script) that runs `mkdir -p` for them. `TF_DATA_DIR` directories are created by Terraform on demand.

{% hint style="info" %}
This only speeds up module and provider downloads. Modules published in the Terrakube private registry are additionally cached in the Terrakube storage and served from there, see [Private Registry](../private-registry/README.md).
{% endhint %}
