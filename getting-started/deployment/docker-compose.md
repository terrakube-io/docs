# Docker Compose

### Step 1 - Clone Docker Compose Repository

```
git clone https://github.com/AzBuilder/azb-docker-compose.git
cd local
docker-compose up
```

> Only use docker compose for testing, all the security is disable and it uses an H2 in-memory database. The terraform registry is not enable because it require a https certificate in order to work with the terraform CLI.

### Step 2 - Test Terrakube Postman Collection.

To test the platform in your local environment you can follow these steps:

1. Download the [postman project](https://github.com/AzBuilder/terrakube-server/tree/main/postman)
2. Create an organization
3. Create a team
4. Create a workspace and fill all the variables or environment variables.
5. Create a job using TCL [(Terrakube Configuration Language)](https://github.com/AzBuilder/terrakube-extensions)

###

\
