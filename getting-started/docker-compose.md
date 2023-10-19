# 🌐 Docker Compose

### Local DNS entries

Update the /etc/hosts file adding the following entries:

```bash
127.0.0.1 terrakube-api
127.0.0.1 terrakube-ui
127.0.0.1 terrakube-executor
127.0.0.1 terrakube-dex
127.0.0.1 terrakube-registry
```

### Running Terrakube Locally.

```bash
git clone https://github.com/AzBuilder/terrakube.git
cd terrakube/docker-compose
docker-compose up -d
```

Terrakube will be available in the following URL:

* http://terrakube-ui:3000
  * Username: admin@example.com
  * Password: admin

### Docker Compose with Open Telemetry Setup

You can also test Terrakube using the Open Telemetry example setup to see how the components are working internally and check some internal logs

```
git clone https://github.com/AzBuilder/terrakube.git
cd terrakube/telemetry-compose
docker-compose up -d
```

{% hint style="info" %}
The setup is using jaegertracing/all-in-one

Jaeger UI will be available in http://localhost:16686/
{% endhint %}

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
For more information about Open Telemetry setup check the [following information](deployment/open-telemetry.md)
{% endhint %}
