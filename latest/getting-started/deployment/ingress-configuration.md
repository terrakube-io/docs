# Ingress Configuration

The default ingress configuration for terrakube can be customize using a terrakube.yaml like the following:

```
dex:
  config:
    issuer: http://terrakube-api.minikube.net/dex ## CHANGE THIS DOMAIN
    #.....
    #PUT ALL THE REST OF THE DEX CONFIG AND UPDATE THE URL WITH THE NEW DOMAIN
    #.....

## Ingress properties
ingress:
  useTls: false
  ui:
    enabled: true
    domain: "terrakube-ui.minikube.net" ## CHANGE THIS DOMAIN
    path: "/"
    pathType: "Prefix" 
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
  api:
    enabled: true
    domain: "terrakube-api.minikube.net" ## CHANGE THIS DOMAIN
    path: "/"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
  registry:
    enabled: true
    domain: "terrakube-reg.minikube.net" ## Change to your customer domain
    path: "/"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
  dex:
    enabled: true
    path: "/dex/"
    pathType: "Prefix"
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/use-regex: "true"
      nginx.ingress.kubernetes.io/configuration-snippet: "proxy_set_header Authorization $http_authorization;"
```

{% hint style="info" %}
Update the custom domains and the other ingress configuration depending of your kubernetes environment
{% endhint %}

{% hint style="warning" %}
The above example is using a simple ngnix ingress
{% endhint %}

Now you can install terrakube using the command

```bash
helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube
```
