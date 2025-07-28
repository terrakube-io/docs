# Dev Container

This page contains the configuration for a development container that provides a consistent environment for working with Terrakube.&#x20;

The devcontainer includes all the necessary tools and dependencies to develop both the Java backend, TypeScript frontend components and includes terraform CLI.

> Make sure The below was tested using Ubuntu-based distribution, not sure if this will work with macos, windows or codespaces

### Features

* Java 17 (Liberica)
* Maven 3.9.9
* Node.js 20.x with Yarn
* VS Code extensions for Java, JavaScript/TypeScript

### Getting Started

#### Prerequisites

* [Visual Studio Code](https://code.visualstudio.com/)
* [VS Code Remote - Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

**Local Development Domains**

To use the devcontainer we need to setup the following domains in our local computer:

```
terrakube.platform.local
terrakube-api.platform.local
terrakube-registry.platform.local
terrakube-dex.platform.local
```

**HTTPS Local Certificates**

Install [mkcert](https://github.com/FiloSottile/mkcert#installation) to generate the local certificates.

To generate local CA certificate execute the following:

```
mkcert -install
Created a new local CA 💥
The local CA is now installed in the system trust store! ⚡️
The local CA is now installed in the Firefox trust store (requires browser restart)! 🦊
```

**Create Docker Network for the devcontainer**

```
docker network create terrakube-network -d bridge --subnet 10.25.25.0/24 --gateway 10.25.25.254
```

We will be using `10.25.25.253` for our the traefik gateway

**Local DNS entries**

Update the /etc/hosts file adding the following entries:

```
10.25.25.253 terrakube.platform.local
10.25.25.253 terrakube-api.platform.local
10.25.25.253 terrakube-registry.platform.local
10.25.25.253 terrakube-dex.platform.local
```

#### Opening the Project in a Dev Container

*   Clone the Terrakube repository and run the project:

    ```
    git clone https://github.com/AzBuilder/terrakube.git
    cd terrakube/.devcontainer
    mkcert -key-file key.pem -cert-file cert.pem platform.local *.platform.local
    CAROOT=$(mkcert -CAROOT)/rootCA.pem
    cp $CAROOT rootCA.pem
    cd ..
    code .
    ```

1. When prompted to "Reopen in Container", click "Reopen in Container". Alternatively, you can:
   * Press F1 or Ctrl+Shift+P
   * Type "Remote-Containers: Reopen in Container" and press Enter
2. Wait for the container to build and start. This may take a few minutes the first time.
3.  Start all Terrakube components

    [![image](https://private-user-images.githubusercontent.com/4461895/444196515-34a4d4c9-d1b0-443f-834e-c4d76db26187.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTc2MzMsIm5iZiI6MTc1MzcxNzMzMywicGF0aCI6Ii80NDYxODk1LzQ0NDE5NjUxNS0zNGE0ZDRjOS1kMWIwLTQ0M2YtODM0ZS1jNGQ3NmRiMjYxODcucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDcyOCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA3MjhUMTU0MjEzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NTYxNmE0NjJjNmYzNzVhMDExYWExM2E1MGNhM2Q1YzhkYmQ5MjhmMGM4OGUwNDYwMzQ5NWY2NTE5MWVkYzQ0NyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.sYgHUp5kaSsse6EL6XXFncvCTW-jveq4eFqvfrdnRI0)](https://private-user-images.githubusercontent.com/4461895/444196515-34a4d4c9-d1b0-443f-834e-c4d76db26187.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTc2MzMsIm5iZiI6MTc1MzcxNzMzMywicGF0aCI6Ii80NDYxODk1LzQ0NDE5NjUxNS0zNGE0ZDRjOS1kMWIwLTQ0M2YtODM0ZS1jNGQ3NmRiMjYxODcucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDcyOCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA3MjhUMTU0MjEzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NTYxNmE0NjJjNmYzNzVhMDExYWExM2E1MGNhM2Q1YzhkYmQ5MjhmMGM4OGUwNDYwMzQ5NWY2NTE5MWVkYzQ0NyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.sYgHUp5kaSsse6EL6XXFncvCTW-jveq4eFqvfrdnRI0)
4.  Terrakube should be availabe with the following url `https://terrakube.platform.local` using `admin@example.com` with password `admin`



    <figure><img src="https://private-user-images.githubusercontent.com/4461895/444197287-c92b5f7a-c484-47b5-bb31-4edd4513278e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTc2MzMsIm5iZiI6MTc1MzcxNzMzMywicGF0aCI6Ii80NDYxODk1LzQ0NDE5NzI4Ny1jOTJiNWY3YS1jNDg0LTQ3YjUtYmIzMS00ZWRkNDUxMzI3OGUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDcyOCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA3MjhUMTU0MjEzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzI5ZTgxYTliZGI2NjdjY2Q4NmRhMWE4ODRkZjYzNTUzMzhiYjg0MWQwZmRmM2NkNGYxZjEwNWEwNzFmNDhjMCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.gNl8kp021D8YOexDQYDCXbSxieHg9mustSpaT_AJpUM" alt=""><figcaption></figcaption></figure>

### Ports

The devcontainer forwards the following ports:

* 8080: Terrakube API
* 8075: Terrakube Registry
* 8090: Terrakube Executor
* 3000: Terrakube UI
* 80/443: Traefik Gateway

### Customization

You can customize the devcontainer by modifying:

* `.devcontainer/devcontainer.json`: VS Code settings and extensions
* `.devcontainer/Dockerfile`: Container image configuration
