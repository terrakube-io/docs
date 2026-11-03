# PostgreSQL

{% hint style="info" %}
By default the helmchart deploy a postgresql database in your terrakube namespace but if you want to customize that you can change the default deployment values.
{% endhint %}

To use a PostgreSQL with your Terrakube deployment create a terrakube.yaml file with the following content:

```yaml
## Terrakube API properties
api:
  defaultDatabase: false
  loadSampleData: false
  properties:
    databaseType: "POSTGRESQL"
    databaseHostname: "server_name.postgres.database.com"
    databaseName: "database_name"
    databaseUser: "database_user"
    databasePassword: "database_password"
    databaseSslMode: "disable"
    databasePort: "5432"

```

{% hint style="info" %}
loadSampleData this will add some organization, workspaces and modules by default in your database if need it
{% endhint %}

Now you can install terrakube using the command.

```bash
helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube
```

{% hint style="warning" %}
Postgresql SSL mode can be use adding databaseSslMode parameter by default the value is "disable", but it accepts the following values; disable, allow, prefer, require, verify-ca, verify-full. This feature is supported from Terrakube 2.15.0. Reference: [https://jdbc.postgresql.org/documentation/publicapi/org/postgresql/PGProperty.html#SSL\_MODE](https://jdbc.postgresql.org/documentation/publicapi/org/postgresql/PGProperty.html#SSL\_MODE)
{% endhint %}
