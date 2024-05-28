# Quick start

### Hello Actions

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

At this moment, our action only interacts with Terrakube data. However, a common requirement is to access other APIs to get some data, for example, to connect to the GitHub API to get the issues related to the workspace, or to connect to a cloud provider to interact with their APIs.

In these cases, we can make use of the Action Proxy. The proxy provides a way to connect to other backend services without needing to directly configure CORS for the API. The proxy also provides a secure way to use credentials without exposing sensitive data to the client.

Let's add a simple interaction using mock data from the JSONPlaceholder API that reads the comments from a post.

```javascript
({ context }) => {
  const [dialogVisible, setDialogVisible] = useState(false);
  const [loading, setLoading] = useState(false);
  const [comments, setComments] = useState([]);

  const fetchData = async () => {
    setLoading(true);
    try {
      const response = await axiosInstance.get(`${context.apiUrl}/proxy/v1`, {
        params: {
          targetUrl: 'https://jsonplaceholder.typicode.com/posts/1/comments',
          proxyheaders: JSON.stringify({
            'Content-Type': 'application/json',
          }),
          workspaceId: context.workspace.id
        }
      });

      setComments(response.data);
      setDialogVisible(true);
    } catch (error) {
      console.error('Error fetching data:', error);
      message.error('Error fetching data');
    } finally {
      setLoading(false);
    }
  };

  const closeDialog = () => {
    setDialogVisible(false);
  };

  return (
    <>
      <Button
        type="default"
        onClick={fetchData}
        loading={loading}
      >
        Quick Start
      </Button>

      <Modal
        title="Comments"
        visible={dialogVisible}
        onCancel={closeDialog}
        footer={[
          <Button key="close" onClick={closeDialog}>
            Close
          </Button>,
        ]}
      >
        {comments.map((comment) => (
          <div key={comment.id} style={{ marginBottom: '10px' }}>
            <p><b>ID:</b> {comment.id}</p>
            <p><b>Name:</b> {comment.name}</p>
            <p><b>Email:</b> {comment.email}</p>
            <p><b>Comment:</b> {comment.body}</p>
          </div>
        ))}
      </Modal>
    </>
  );
};
```

Now if you click the Quick Start button, you will see a dialog with the comments data from the API. The proxy allows to interact with external apis and securely manage credentials. To know more about the Actions proxy check the documentation.

<figure><img src="../../../../.gitbook/assets/image (397).png" alt=""><figcaption></figcaption></figure>

Now, if you click the `Quick Start` button, you will see a dialog with the comments data from the API. The proxy allows you to interact with external APIs and securely manage credentials. To learn more about the Actions proxy, check the [documentation](action-proxy.md).

### Summary

Actions in Terrakube allow you to enhance the Workspace experience by creating custom interactions and functionalities tailored to your needs. You can start by exploring the [Built-in Actions ](../built-in-actions/)to understand their implementation and gain more experience.

Actions are React components, and you define an [Action Type](action-types.md) to determine where the action will appear within the interface. Using [Display Criteria](display-criteria.md), you can specify the precise conditions under which an action should be displayed, such as for specific organizations or Terraform versions.

Additionally, the [Action Proxy](action-proxy.md) enables integration with external APIs, securely managing credentials without requiring CORS configuration on the Terrakube front end. This is particularly useful when you don't have control over the external API's CORS settings.&#x20;
