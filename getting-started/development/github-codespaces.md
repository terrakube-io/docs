# GitHub Codespaces

This page contains the configuration for a development container using GitHub Codespaces that provides a consistent environment for working with Terrakube.

The devcontainer includes all the necessary tools and dependencies to develop both the Java backend, TypeScript frontend components and includes terraform CLI.

### Features

* Java 25 (Liberica)
* Maven 3.9.9
* Node.js 24.x with Yarn
* VS Code extensions for Java, JavaScript/TypeScript

### Prerequisites

* GitHub Account

### **Prepare project**

The first step is to create a GitHub fork to work with Terrakube

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Once you have created a fork you need to create a new branch to work for any new Terrakube feature

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

Open a new GitHub workspace using "new with options"

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

Select a 4 CPU configuration like the following.

<figure><img src="../../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>

Dev container setup will take a couple of minutes.

<figure><img src="../../.gitbook/assets/image (295).png" alt=""><figcaption></figcaption></figure>

Once the environment setup is completed, you will see the following message

<figure><img src="../../.gitbook/assets/image (302).png" alt=""><figcaption></figcaption></figure>

Now we can run the new development environment

<figure><img src="../../.gitbook/assets/image (315).png" alt=""><figcaption></figcaption></figure>

You need to accept using the Java "Standard Mode".

<figure><img src="../../.gitbook/assets/image (318).png" alt=""><figcaption></figcaption></figure>

The dev container will download all the maven dependencies, this could take a couple of minutes

<figure><img src="../../.gitbook/assets/image (398).png" alt=""><figcaption></figcaption></figure>

Once all dependencies are downloaded, you can see the four component running like the following

<figure><img src="../../.gitbook/assets/image (406).png" alt=""><figcaption></figcaption></figure>

If not you can simply start the ui, api, registry and executor one by one using the following menu

<figure><img src="../../.gitbook/assets/image (407).png" alt=""><figcaption></figcaption></figure>

After we have the components running, we need to change the port configuration, we need to make sure DEX, the api, registry and ui are using public ports.

<figure><img src="../../.gitbook/assets/image (427).png" alt=""><figcaption></figcaption></figure>

Example:

<figure><img src="../../.gitbook/assets/image (442).png" alt=""><figcaption></figcaption></figure>

In the end it should look like this using public ports:

<figure><img src="../../.gitbook/assets/image (484).png" alt=""><figcaption></figcaption></figure>

The development environment automatically creates "DEVCONTAINER.md" that contains all the URL for each component.

<figure><img src="../../.gitbook/assets/image (485).png" alt=""><figcaption></figcaption></figure>

You will need to visit the URL for dex, ui, api and registry in order to accept that we need to use a public port for our environment.

<figure><img src="../../.gitbook/assets/image (486).png" alt=""><figcaption></figcaption></figure>

You will see a message like this and click "continue"

<figure><img src="../../.gitbook/assets/image (487).png" alt=""><figcaption></figcaption></figure>

Once you click continue

<figure><img src="../../.gitbook/assets/image (488).png" alt=""><figcaption></figcaption></figure>

You need to do the same for the other components: api, registry, executor and dex

<figure><img src="../../.gitbook/assets/image (489).png" alt=""><figcaption></figcaption></figure>

Now you can go back to the UI and click "Sign in"

<figure><img src="../../.gitbook/assets/image (490).png" alt=""><figcaption></figcaption></figure>

This will redirect you to dex where you can use "admin@example.com" and "admin" to complete the login.

<figure><img src="../../.gitbook/assets/image (491).png" alt=""><figcaption></figcaption></figure>

Grant DEX access

<figure><img src="../../.gitbook/assets/image (492).png" alt=""><figcaption></figcaption></figure>

Now you can start testing the development environment and do any required changes

<figure><img src="../../.gitbook/assets/image (493).png" alt=""><figcaption></figcaption></figure>

If you need to update the code for any java component, you can simply click "Stop"

<figure><img src="../../.gitbook/assets/image (494).png" alt=""><figcaption></figcaption></figure>

Start a new instance

<figure><img src="../../.gitbook/assets/image (495).png" alt=""><figcaption></figcaption></figure>

Once all the changes are completed, we pick the files an push the changes to our feature branch

<figure><img src="../../.gitbook/assets/image (496).png" alt=""><figcaption></figcaption></figure>

You can return to GitHub and create a new pull request to the Terrakube main repository

<figure><img src="../../.gitbook/assets/image (497).png" alt=""><figcaption></figcaption></figure>

In the end the pull request will look like this.

<figure><img src="../../.gitbook/assets/image (498).png" alt=""><figcaption></figcaption></figure>

Now you have completed your open source contribution to Terrakube 🙂

Finally you can go to GitHub and delete your codespace.

<figure><img src="../../.gitbook/assets/image (499).png" alt=""><figcaption></figcaption></figure>
