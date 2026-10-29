# Custom CA Certs

Use this when Terrakube must trust a private certificate authority, for example for an internal Git server, an internal registry, a TLS-inspecting proxy, or a database or Redis with a private certificate. A missing CA shows up as `x509: certificate signed by unknown authority`.

### With the Helm chart

Put the PEM certificates under `security.caCerts` in your values file:

```yaml
security:
  caCerts:
    internal-root-ca.pem: |
      -----BEGIN CERTIFICATE-----
      MIIDZTCCA...
      -----END CERTIFICATE-----
    proxy-ca.pem: |
      -----BEGIN CERTIFICATE-----
      MIIDZTCCA...
      -----END CERTIFICATE-----
```

Then `helm upgrade`. The chart stores the certificates in a Kubernetes secret (`terrakube-ca-secrets`), mounts them into the **API, registry and executor** pods, and sets `SERVICE_BINDING_ROOT`, so there is nothing else to configure. This needs chart 4.2.6 or later. Restart the pods if they do not roll on their own.

To confirm, check the start-up log of a pod for a line like:

```text
Added 2 additional CA certificate(s) to system truststore
```

### How it works

The Terrakube images are built with Cloud Native Buildpacks. At start-up the buildpack reads certificates from `$SERVICE_BINDING_ROOT/ca-certificates`, which must contain your `.pem` files and a file named `type` whose content is `ca-certificates`, and adds them to the JVM and system trust stores. The chart creates the `type` file for you. Without the chart, mount the same folder yourself at `/mnt/platform/bindings/ca-certificates` and set `SERVICE_BINDING_ROOT=/mnt/platform/bindings`.

### At build time

If you build your own images, put the certificates in `bindings/ca-certificates` of a checkout of the [terrakube repository](https://github.com/terrakube-io/terrakube) and run `scripts/build/terrakubeBuild.sh` from the root.

### Related

* A corporate proxy: [Proxy Configuration](proxy-configuration.md).
* Redis with a private or managed certificate: [External Redis](external-redis.md).
* Symptoms and fixes: [Troubleshooting](../../user-guide/reference/troubleshooting.md#a-step-failed-with-x509-certificate-signed-by-unknown-authority).
