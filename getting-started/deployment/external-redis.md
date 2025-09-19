# External Redis

By default the helm chart is using bitnami redis, this container is only for testing purposes and for a real cluster it is better to use an external redis.

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
  version: "2.23.2"
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
