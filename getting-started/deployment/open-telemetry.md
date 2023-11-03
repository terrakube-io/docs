# 🚦 Open Telemetry

Terrakube components support [Open Telemetry](https://opentelemetry.io/) by default to enable effective observability.

To enable telemetry inside the Terrakube components please add the following environment variable:

```
OTEL_JAVAAGENT_ENABLE=true
```

{% hint style="success" %}
Terrakube API, Registry and Executor support the setup for now from version 2.12.0.

UI support will be added in the future.
{% endhint %}

Once the open telemetry agent is enable we can use other environment variables to setup the monitoring for our application for example to enable jaeger we could add the following using addtional environment variables:

```
OTEL_TRACES_EXPORTER=jaeger
OTEL_EXPORTER_JAEGER_ENDPOINT=http://jaeger-all-in-one:14250
OTEL_SERVICE_NAME=TERRAKUBE-API
```

Now we can go the jaeger ui to see if everything is working as expected.

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

There are several differente configuration options for example:

#### Jaeger exporter

The [Jaeger](https://www.jaegertracing.io/docs/1.21/apis/#protobuf-via-grpc-stable) exporter. This exporter uses gRPC for its communications protocol.

| System property               | Environment variable             | Description                                                                                |
| ----------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------ |
| otel.traces.exporter=jaeger   | OTEL\_TRACES\_EXPORTER=jaeger    | Select the Jaeger exporter                                                                 |
| otel.exporter.jaeger.endpoint | OTEL\_EXPORTER\_JAEGER\_ENDPOINT | The Jaeger gRPC endpoint to connect to. Default is `http://localhost:14250`.               |
| otel.exporter.jaeger.timeout  | OTEL\_EXPORTER\_JAEGER\_TIMEOUT  | The maximum waiting time, in milliseconds, allowed to send each batch. Default is `10000`. |

#### Zipkin exporter

The [Zipkin](https://zipkin.io/zipkin-api/) exporter. It sends JSON in [Zipkin format](https://zipkin.io/zipkin-api/#/default/post\_spans) to a specified HTTP URL.

| System property               | Environment variable             | Description                                                                                                           |
| ----------------------------- | -------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| otel.traces.exporter=zipkin   | OTEL\_TRACES\_EXPORTER=zipkin    | Select the Zipkin exporter                                                                                            |
| otel.exporter.zipkin.endpoint | OTEL\_EXPORTER\_ZIPKIN\_ENDPOINT | The Zipkin endpoint to connect to. Default is `http://localhost:9411/api/v2/spans`. Currently only HTTP is supported. |

#### Prometheus exporter

The [Prometheus](https://github.com/prometheus/docs/blob/master/content/docs/instrumenting/exposition\_formats.md) exporter.

| System property                  | Environment variable               | Description                                                                        |
| -------------------------------- | ---------------------------------- | ---------------------------------------------------------------------------------- |
| otel.metrics.exporter=prometheus | OTEL\_METRICS\_EXPORTER=prometheus | Select the Prometheus exporter                                                     |
| otel.exporter.prometheus.port    | OTEL\_EXPORTER\_PROMETHEUS\_PORT   | The local port used to bind the prometheus metric server. Default is `9464`.       |
| otel.exporter.prometheus.host    | OTEL\_EXPORTER\_PROMETHEUS\_HOST   | The local address used to bind the prometheus metric server. Default is `0.0.0.0`. |

{% hint style="success" %}
For more information please check, [the official open telemetry documentation](https://github.com/open-telemetry/opentelemetry-java/blob/main/sdk-extensions/autoconfigure/README.md).
{% endhint %}

Open Telemetry Example

One small example to show how to use open telemetry with docker compose can be found in the following URL:

{% embed url="https://github.com/AzBuilder/terrakube/tree/main/telemetry-compose" %}
