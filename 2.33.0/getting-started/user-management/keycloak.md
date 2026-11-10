# Keycloak

#### Requirements

* A working Keycloak server with a configured realm.

#### Steps for configuring Terrakube with Keycloak



For configuring Terrakube with Keycloak using Dex the following steps are involved:

* Keycloak client creation and configuration for Terrakube.
* Configure Terrakube so it works with Keycloak.
* Testing the configuration.

#### Keycloak client creation



Log in to Keycloak with admin credentials and select `Configure > Clients > Create`. Define the ID as `TerrakubeClient` and select `openid-connect` as its protocol. For `Root URL` use Terrakube's GUI URL. Then click `Save`.

<figure><img src="https://github.com/AzBuilder/docs/raw/main/.gitbook/assets/integration_with_keycloak/keycloak_terrakube_client_1.png" alt=""><figcaption></figcaption></figure>

A form for finishing the Terrakube client configuration will be displayed. These are the fields that must be fulfilled:

* **Name:** in this example it has the value of `TerrakubeClient`.
* **Client Protocol:** it must be set to `openid-connect`.
* **Access Type:** set it to `confidential`.
* **Root URL:** Terrakube's UI URL.
* **Valid Redirect URIs:** set it to `*`

Then click `Save`.

<figure><img src="https://github.com/AzBuilder/docs/raw/main/.gitbook/assets/integration_with_keycloak/keycloak_terrakube_client_2.png" alt=""><figcaption></figcaption></figure>

Notice that, since we set `Access Type` to `confidential`, we have an extra tab titled `Credentials`. Click the `Credentials` tab and copy the **Secret** value. It will be used later when we configure the Terrakube's connector.

<figure><img src="https://github.com/AzBuilder/docs/raw/main/.gitbook/assets/integration_with_keycloak/keycloak_terrakube_client_3.png" alt=""><figcaption></figcaption></figure>

Depending on your configuration, Terrakube might expect different client scopes, such as `openid`, `profile`, `email`, `groups`, etc. You can see if they are assigned to `TerrakubeClient` by clicking on the `Client Scopes` tab (in `TerrakubeClient`).

If they are not assigned, you can assign them by selecting the scopes and clicking on the `Add selected` button.

<figure><img src="https://github.com/AzBuilder/docs/raw/main/.gitbook/assets/integration_with_keycloak/keycloak_terrakube_client_4.png" alt=""><figcaption></figcaption></figure>

If some scope does not exist, you must create it before assigning it to the client. You do this by clicking on `Client Scopes`, then click on the `Create` button. This will lead you to a form where you can create the new scope. Then, you can assign it to the client.

<figure><img src="https://github.com/AzBuilder/docs/raw/main/.gitbook/assets/integration_with_keycloak/keycloak_terrakube_client_5.png" alt=""><figcaption></figcaption></figure>

#### Terrakube configuration



We have to configure a Dex connector to use with Keycloak. Add a new connector in Dex's configuration, so it looks like this:

```
      connectors: 
        - type: oidc
          id: TerrakubeClient
          name: TerrakubeClient
          config:
            issuer: "[http|https]://<KEYCLOAK_SERVER>/realms/<MY_REALM>"
            clientID: "TerrakubeClient"
            clientSecret: "<TerrakubeClient's secret>"
            redirectURI: "[http|https]://<TERRAKUBE-API URL>/dex/callback"
            insecureEnableGroups: true
```

This is the simpler configuration that we can use. Let's see some notes about this fields:

* **type:** must be `oidc` (OpenID Connect).
* **name:** this is the string shown in the connectors list in Terrakube GUI.
* **issuer:** it refers to the Keycloak server. It has the form `[http|https]://<KEYCLOAK_SERVER>/realms/<REALM_NAME>`
* **clientID:** refers to the Client ID configured in Keycloak. They must match.
* **clientSecret:** must be the secret in the Credentials tab in Keycloak..
* **redirectURI:** has the form `[http|https]://<TERRAKUBE_API>/dex/callback`. Notice this is the **Terrakube API** URL and not the UI URL.
* **insecureEnableGroups:** this is required to enable groups claims. This way groups defined in Keycloak are brought by Terrakube's Dex connector.

\{% hint style="info" %\} If your users do not have a name set (`First Name` field in Keycloak), you must tell oidc which attribute to use as the user's name. You can do this giving the `userNameKey`:

```
config:
  .....
  userNameKey: email
```

\{% endhint %\}

#### Testing Terrakube authentication



When we click on Terrakube's login button we are given the choice to select the connector we want to use:

<figure><img src="https://github.com/AzBuilder/docs/raw/main/.gitbook/assets/integration_with_keycloak/keycloak_terrakube_login_1.png" alt=""><figcaption></figcaption></figure>

Click on `Log in with TerrakubeClient`. You will be redirected to a login form in Keycloak:

<figure><img src="https://github.com/AzBuilder/docs/raw/main/.gitbook/assets/integration_with_keycloak/keycloak_terrakube_login_2.png" alt=""><figcaption></figcaption></figure>

After login, you are redirected back to Terrakube and a dialog asking you to grant access is shown

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Click on `Grant Access`. That is the last step. If everything went right, now you should be logged in Terrakube.

Dex can be further configured by setting `dex.config.web.skipApprovalScreen` to avoid granting access everytime.
It is also recommended to switch to a `database` mode instead of `memory` to avoid forcing all users to re-authenticate
each time dex pod crashes. Switching to `database` mode will also allow you to run several dex pods for better availability.
