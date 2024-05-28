# Quick start

## Hello Actions

Let's start writing our Hello World action. This action will include a single button with the text "Quick Start".

```javascript
({ context }) => {
   return (
      <Button>
         Quick Start
      </Button>
   );
};
```

As you can see, an action is essentially some JavaScript code, specifically a React component. If you are not familiar with JavaScript and React, I recommend [learning the basics of React](https://react.dev/learn) before continuing.

This component contains only one button and receives a `context` parameter. We will discuss more about the `context` later. For now, you only need to know that the `context` provides some data related to the area where the action will appear.

Terrakube uses [Ant Design (antd) components](https://ant.design/components/overview), so by default, all antd components are available when you are developing an action.

### Testing our Hello world action

To create the action in your Terrakube instance, navigate to the Organization settings and click the `Create Action` button

<figure><img src="../../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Add the following data and click the save button.

| Field Name       | Description                                                                         |
| ---------------- | ----------------------------------------------------------------------------------- |
| ID               | terrakube.quick-start                                                               |
| Name             | Quick Start                                                                         |
| Type             | Workspace/Action                                                                    |
| Label            | Quick Start                                                                         |
| Category         | General                                                                             |
| Version          | 1.0.0                                                                               |
| Description      | Quick Start action                                                                  |
| Display Criteria | Add a new display criteria and enter true. This means the action will appear always |
| Action           | Add the above code                                                                  |
| Active           | Enable                                                                              |

<figure><img src="../../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

If you go to any workspace, the quick start button should appear.

<figure><img src="../../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### Adding Interaction to the Action

Now let's add some interaction to the button. In the next example, we will use the Ant Design Messages component.

```javascript
({ context }) => {
  const [messageApi, contextHolder] = message.useMessage();

  const showMessage = () => {
    messageApi.success('Hello Actions');
  };

  return (
    <>
      {contextHolder}
      <Button onClick={showMessage}>
        Quick Start
      </Button>
    </>
  );
};
```

Now, if you update the action code, navigate to any workspace, and click the `Quick Start` button, you will see the `Hello Actions` message.

<figure><img src="../../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### Using the context to access workspace data

To make this action more useful, you can make use of the context. The context is an object that contains data based on the [Action Type](action-types.md). If you want to see all the data available for every action type, check the [Action Context documentation](action-context.md).

For our quick start action, we will add the name of the workspace to the message.

```javascript
({ context }) => {
  const [messageApi, contextHolder] = window.antd.message.useMessage();

  const showMessage = () => {
    messageApi.success(`Hello ${context.workspace.attributes.name}`);
  };

  return (
    <>
      {contextHolder}
      <Button onClick={showMessage}>
        Quick Start
      </Button>
    </>
  );
};
```

Update the action code using the code above, click the `Quick Start` button, and you will see the message now displays the Workspace Name.

<figure><img src="../../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### Using display criteria

f you view a couple of workspaces in the organization, you will see the "Quick Start" button always appears. This is useful if your action serves a general purpose, but there are scenarios where you want to display the action only for workspaces in a specific organization, with a specific version of Terraform, and so on.

In those cases, you will use display criteria, which provides a powerful way to decide when an action will appear or not. If you remember, we set the display criteria to `true` before. Now let's modify the criteria to show the action only if the workspace contains a specific name.

Navigate to the Actions settings and change the display criteria to:

```javascript
context.workspace.attributes.name === "sample_simple"
```

(You can use another workspace name if you don't have the "sample\_simple" workspace.)

Now the button will appear only for the `sample_simple` workspace. If you notice, the display criteria is a conditional JavaScript code. So you can use any JavaScript code to display the action based on a regular expression, for organizations starting with certain names, or even combine multiple conditions.

<figure><img src="../../../../.gitbook/assets/image (16).png" alt=""><figcaption><p>Button appears</p></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (17).png" alt=""><figcaption><p>Button doesn't appear</p></figcaption></figure>

### Accessing external APIs

At this moment our action only interacts with the same Terrakube data. But a common requirement is to access other apis to get some data, for example to connect to the Gtibhu api to get the issues related to the workspace, or connect to a Cloud provider to interact with their apis.&#x20;

In this cases we can make use of the Action Proxy. The proxy provides a way to connect to other backend service without need to directly configure CORS for the API. The proxy also provides a secure way to use credentials without the need that the client knows the sensitive data.

Lets add a simple interaction
