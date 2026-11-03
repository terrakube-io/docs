# External Redis

By default the Helm chart installs a bundled Valkey (a Redis-compatible store). It is for testing only; in a real cluster use an external Redis or Valkey, such as Azure Cache for Redis, Amazon ElastiCache or Memorystore. Redis holds the scheduler's locks and the live log stream, so it is required when you run more than one API or executor replica.

To enable it, the following values can be used:

```yaml
api:
  defaultRedis: false
  env:
  - name: TerrakubeRedisSSL
    value: "true"
  - name: TerrakubeRedisTruststorePath
    value: /layers/paketo-buildpacks_bellsoft-liberica/jre/lib/security/cacerts 
  - name: TerrakubeRedisTruststorePassword
    value: changeit
  properties:
    redisHostname: "MY-REDIS-IN-AZURE.redis.cache.windows.net"
    redisPassword: "MY-REDIS-ACCESS-KEY"
    redisPort: "6380"

executor:
  env:
  - name: SERVICE_BINDING_ROOT
    value: /mnt/platform/bindings
  - name: TerrakubeRedisSSL
    value: "true"
  - name: TerrakubeRedisTruststorePath
    value: /layers/paketo-buildpacks_bellsoft-liberica/jre/lib/security/cacerts 
  - name: TerrakubeRedisTruststorePassword
    value: changeit
```

{% hint style="warning" %}
"changeit" is the default password for the java keystore included in the container in this path "/layers/paketo-buildpacks\_bellsoft-liberica/jre/lib/security/cacerts"
{% endhint %}

### Ephemeral Executor with Redis SSL

{% hint style="info" %}
Using EPHEMERAL\_JOB\_ENV\_VARS is supported from version 2.28.0
{% endhint %}

To add the additional environment variables to the ephemeral executor the following could be used:

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Value:

```
EPHEMERAL_JOB_ENV_VARS=TerrakubeRedisSSL=true;TerrakubeRedisTruststorePath=/layers/paketo-buildpacks_bellsoft-liberica/jre/lib/security/cacerts;TerrakubeRedisTruststorePassword=changeit;ExecutorEphemeralSecret=terrakube-executor-secrets-ephemeral
```
