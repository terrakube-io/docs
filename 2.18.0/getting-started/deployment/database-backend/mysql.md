# MySQL

To use a MySQL with your Terrakube deployment create a terrakube.yaml file with the following content:

```yaml
## Terrakube API properties
api:
  defaultDatabase: false
  loadSampleData: false
  properties:
    databaseType: "MYSQL"
    databaseHostname: "server_name.database.mysql.com"
    databaseName: "database_name"
    databaseUser: "database_user"
    databasePassword: "database_password"

```

{% hint style="info" %}
loadSampleData this will add some organization, workspaces and modules by default in your database
{% endhint %}

Now you can install terrakube using the command.

```bash
helm install --values terrakube.yaml terrakube terrakube-repo/terrakube -n terrakube
```
