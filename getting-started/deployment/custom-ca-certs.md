# 🔐 Custom CA Certs

Terrakube componentes (api, registry and executor) are using [buildpacks](https://buildpacks.io/docs/concepts/) to create the docker images

<figure><img src="https://user-images.githubusercontent.com/4461895/223739868-d206fa74-4a06-40eb-9790-db36df6ee74e.png" alt=""><figcaption></figcaption></figure>

When using buildpack to add a custom CA certificate at runtime you need to do the following:

Provide the following environment variable to the container:

```
SERVICE_BINDING_ROOT: /mnt/platform/bindings
```

Inside the path there is a folder call "ca-certificates"

```
cnb@terrakube-api-678cb68d5b-ns5gt:/mnt/platform/bindings$ ls
ca-certificates
```

We need to mount some information to that path

```
/mnt/platform/bindings/ca-certificates
```

Inside this folder we should put out custom PEM CA certs and one additional file call **type**

```
cnb@terrakube-api-678cb68d5b-ns5gt:/mnt/platform/bindings/ca-certificates$ ls
terrakubeDemo1.pem  terrakubeDemo2.pem  type
```

The content of the file **type** is just the text **"ca-certificates"**

```
cnb@terrakube-api-678cb68d5b-ns5gt:/mnt/platform/bindings/ca-certificates$ cat type
ca-certificates
```

Finally your helm terrakube.yaml should look something like this because we are mounting out CA certs and the file called **type** in the following path **" /mnt/platform/bindings/ca-certificates"**

```
## Terrakube Security
security:
  caCerts:
    terrakubeDemo1.pem: |
      -----BEGIN CERTIFICATE-----
      MIIDZTCCA.........

      -----END CERTIFICATE-----
    terrakubeDemo2.pem: |
      -----BEGIN CERTIFICATE-----
      MIIDZTCCA.....
      

      -----END CERTIFICATE-----

## API properties
api:
  version: "2.11.2"
  env:
  - name: SERVICE_BINDING_ROOT
    value: /mnt/platform/bindings
  volumes:
    - name: ca-certs
      secret:
        secretName: terrakube-ca-secrets
        items:
        - key: "terrakubeDemo1.pem"
          path: "terrakubeDemo1.pem"
        - key: "terrakubeDemo2.pem"
          path: "terrakubeDemo2.pem"
        - key: "type'
          path: "type"
  volumeMounts:
  - name: ca-certs
    mountPath: /mnt/platform/bindings/ca-certificates
    readOnly: true
```

Checking the terrakube component two additional ca certs are added inside the sytem truststore

```
Added 2 additional CA certificate(s) to system truststore
Setting Active Processor Count to 2
Calculating JVM memory based on 5791152K available memory
For more information on this calculation, see https://paketo.io/docs/reference/java-reference/#memory-calculator
Calculated JVM Memory Configuration: -XX:MaxDirectMemorySize=10M -Xmx5033128K -XX:MaxMetaspaceSize=246023K -XX:ReservedCodeCacheSize=240M -Xss1M (Total Memory: 5791152K, Thread Count: 250, Loaded Class Count: 41022, Headroom: 0%)
Enabling Java Native Memory Tracking
Adding 126 container CA certificates to JVM truststore
Spring Cloud Bindings Enabled
Picked up JAVA_TOOL_OPTIONS: -Djava.security.properties=/layers/paketo-buildpacks_bellsoft-liberica/java-security-properties/java-security.properties -XX:+ExitOnOutOfMemoryError -XX:ActiveProcessorCount=2 -XX:MaxDirectMemorySize=10M -Xmx5033128K -XX:MaxMetaspaceSize=246023K -XX:ReservedCodeCacheSize=240M -Xss1M -XX:+UnlockDiagnosticVMOptions -XX:NativeMemoryTracking=summary -XX:+PrintNMTStatistics -Dorg.springframework.cloud.bindings.boot.enable=true

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.8)
```

Additinal information about buildpacks can be found in this link:

* [https://blog.dahanne.net/2021/02/06/customizing-cloud-native-buildpacks-practical-examples/#Add\_certificates\_binding\_at\_runtime:\~:text=ca%2Dcertificates-,Add%20certificates%20binding%20at%20runtime,-If%20your%20image](https://blog.dahanne.net/2021/02/06/customizing-cloud-native-buildpacks-practical-examples/#Add\_certificates\_binding\_at\_runtime)
* [https://github.com/paketo-buildpacks/ca-certificates](https://github.com/paketo-buildpacks/ca-certificates)
* [https://github.com/paketo-buildpacks/spring-boot](https://github.com/paketo-buildpacks/spring-boot)
