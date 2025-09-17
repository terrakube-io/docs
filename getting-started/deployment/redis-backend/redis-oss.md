### Redis

One is recommended to use external redis for production deployment. By default Terrakube helm chart will deploy a redis
on your kubernetes cluster. In order to change the default behavior and use your custom redis you will need to set the
following values

```
  api:
    defaultRedis: false
    env:
    - name: ExecutorEphemeralSecret
      value: terrakube-executor-secrets
    - name: TerrakubeRedisSSL
      value: "true"
    - name: TerrakubeRedisTruststorePath
      value: /layers/paketo-buildpacks_bellsoft-liberica/jre/lib/security/cacerts
    - name: TerrakubeRedisTruststorePassword
      value: changeit
```

Note that, it is recommended to put your redis password inside a kubernetes secret and import value from it. In this
example `ExecutorEphemeralSecret` is used for [EphemeralAgents](https://docs.terrakube.io/getting-started/deployment/ephemeral-agents)
