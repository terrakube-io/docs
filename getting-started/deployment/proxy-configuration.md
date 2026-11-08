# Proxy Configuration

When Terrrakube needs to run behind a corporate proxy the following environment variable can be used in each container:

```
JAVA_TOOL_OPTIONS="-Dhttp.proxyHost=your.proxy.net -Dhttp.proxyPort=8080 -Dhttp.nonProxyHosts=XXXXX -Dhttps.proxyHost=your.proxy.net -Dhttps.proxyPort=8080 -Dhttps.nonProxyHosts=XXXXX"
```

When using the official helm chart  the environment variable setting can be set like the following:

```yaml
api:
  env:
  - name: JAVA_TOOL_OPTIONS
    value: "-Dhttp.proxyHost=your.proxy.net -Dhttp.proxyPort=8080 -Dhttp.nonProxyHosts=XXXXX -Dhttps.proxyHost=your.proxy.net -Dhttps.proxyPort=8080 -Dhttps.nonProxyHosts=XXXXX"
    
registry:
  env:
  - name: JAVA_TOOL_OPTIONS
    value: "-Dhttp.proxyHost=your.proxy.net -Dhttp.proxyPort=8080 -Dhttp.nonProxyHosts=XXXXX -Dhttps.proxyHost=your.proxy.net -Dhttps.proxyPort=8080 -Dhttps.nonProxyHosts=XXXXX"

executor:
  env:
  - name: JAVA_TOOL_OPTIONS
    value: "-Dhttp.proxyHost=your.proxy.net -Dhttp.proxyPort=8080 -Dhttp.nonProxyHosts=XXXXX -Dhttps.proxyHost=your.proxy.net -Dhttps.proxyPort=8080 -Dhttps.nonProxyHosts=XXXXX"
```

[Reference](https://github.com/terrakube-io/terrakube/issues/699)
