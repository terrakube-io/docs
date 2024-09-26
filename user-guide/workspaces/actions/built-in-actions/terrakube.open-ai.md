# Open AI

{% hint style="warning" %}
**Security Warning**

The current action shares the state with the OpenAI API, so it's not recommended for sensitive workspaces. Future versions will provide a mask method before sending the data.
{% endhint %}

### Description

The Ask Workspace action allows users to interact with OpenAI's GPT-4o model to ask questions and get assistance related to their Terraform Workspace. This action provides a chat interface where users can ask questions about their Workspace's Terraform state and receive helpful responses from the AI.

### Display Criteria

By default, this Action is disabled and when enabled will appear for all resources. If you want to display this action only for certain resources, please check [display criteria](terrakube.open-ai.md#display-criteria).

### Setup

This action requires the following variables as [Workspace Variables](../../variables.md#workspace-specific-variables) or [Global Variables](../../../organizations/global-variables.md) in the Workspace Organization:

* **OPENAI\_API\_KEY**: The API key to authenticate requests to the OpenAI API. Please check [this guide](https://platform.openai.com/docs/quickstart/step-2-set-up-your-api-key) in how to get an OPEN AI api key

### Usage

* Navigate to the `Workspace` and click on the `Ask Workspace` button.

<figure><img src="../../../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Enter your questions and click the **Send** button or press the **Enter** key.

<figure><img src="../../../../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>
