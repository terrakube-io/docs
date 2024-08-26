# 🔐 User Authentication (DEX)

To authenticate users Terrakube implement [DEX](https://dexidp.io/) so you can authenticate using differente providers  using [dex connectors](https://dexidp.io/docs/connectors/) like the following:

* Azure Active Direcory
* Google Cloud Identity
* Amazon Cognito
* Github Authentication
* Gitlab Authentictaion
* OIDC
* LDAP
* Keycloak
* etc.

{% hint style="info" %}
Any dex connector that implements the "groups" scope should work without any issue
{% endhint %}

The Terrakube [helm chart](https://azbuilder.github.io/terrakube-helm-chart/) is using DEX as dependency, so you can quickly implement it using any exiting dex configuration.

{% hint style="danger" %}
Make sure to update the dex configuration when deploying terrakube in a real kubernetes environment, by default it is using a very basic openLDAP with some sample data. To disable udpate security.useOpenLDAP in your terrakube.yaml
{% endhint %}

To customize the DEX setup just create a simple terrakube.yaml and update the configuration like the following example:

```
# UPDATE THE DEX CLIENT ID AND SCOPE 
security:
  dexClientId: "example-app"
  dexClientScope: "email openid profile offline_access groups"
  useOpenLDAP: false
  
# UPDATE THE DEX CONFIG
dex:
  config:
    issuer: http://terrakube-api.minikube.net/dex

    storage:
      type: memory
    web:
      http: 0.0.0.0:5556
      allowedOrigins: ['*']
      skipApprovalScreen: true
    oauth2:
      responseTypes: ["code", "token", "id_token"] 

    connectors:
    - type: ldap
      name: OpenLDAP
      id: ldap
      config:
        # The following configurations seem to work with OpenLDAP:
        #
        # 1) Plain LDAP, without TLS:
        host: terrakube-openldap-service:389
        insecureNoSSL: true
        #
        # 2) LDAPS without certificate validation:
        #host: localhost:636
        #insecureNoSSL: false
        #insecureSkipVerify: true
        #
        # 3) LDAPS with certificate validation:
        #host: YOUR-HOSTNAME:636
        #insecureNoSSL: false
        #insecureSkipVerify: false
        #rootCAData: 'CERT'
        # ...where CERT="$( base64 -w 0 your-cert.crt )"

        # This would normally be a read-only user.
        bindDN: cn=admin,dc=example,dc=org
        bindPW: admin

        usernamePrompt: Email Address

        userSearch:
          baseDN: ou=People,dc=example,dc=org
          filter: "(objectClass=person)"
          username: mail
          # "DN" (case sensitive) is a special attribute name. It indicates that
          # this value should be taken from the entity's DN not an attribute on
          # the entity.
          idAttr: DN
          emailAttr: mail
          nameAttr: cn

        groupSearch:
          baseDN: ou=Groups,dc=example,dc=org
          filter: "(objectClass=groupOfNames)"

          userMatchers:
            # A user is a member of a group when their DN matches
            # the value of a "member" attribute on the group entity.
          - userAttr: DN
            groupAttr: member

          # The group name should be the "cn" value.
          nameAttr: cn

    staticClients:
    - id: example-app
      redirectURIs:
      - 'http://terrakube-ui.minikube.net'
      - '/device/callback'
      - 'http://localhost:10000/login'
      - 'http://localhost:10001/login'
      name: 'example-app'
      public: true
```

{% hint style="success" %}
Dex configuration examples can be found [here](https://dexidp.io/docs/connectors/).
{% endhint %}

