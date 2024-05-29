# Action Proxy

The Action Proxy allows you to call other APIs without needing to add the Terrakube frontend to the CORS settings, which is particularly useful if you don't have access to configure CORS. Additionally, it can be used to inject Workspace variables and Global variables into your requests.

### **Calling the Action Proxy**

The proxy supports POST, GET, PUT, and DELETE methods. You can access it using the `axiosInstance` variable, which contains an [Axios](https://axios-http.com/docs/intro) object.

To invoke the proxy, use the following URL format: `${context.apiUrl}/proxy/v1`

```javascript
({ context }) => {
   const fetchData = async () => {
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

      console.log(response.data);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
 
  };
```

When calling the Action Proxy, use the following required parameters:

* **targetUrl:** Contains the URL that you want to invoke.
* **proxyHeaders:** Contains the headers that you want to send to the target URL.
* **workspaceId:** The workspace ID that will be used for some validations from the proxy side

### **Injecting Variables via the Action Proxy**

If you need to access sensitive keys or passwords from your API call, you can inject variables using the template notation `${{var.variable_name}}`, where `variable_name` represents a [Workspace variable](../../../terrakube-cli/commands/terrakube-workspace/workspace-variable/) or a[ Global variable](../../../organizations/global-variables.md).

{% hint style="info" %}
If you have a Global variable and a Workspace variable with the same name, the Workspace variable value will take priority.
{% endhint %}

**Example Usage**

```javascript
const response = await axiosInstance.post(`${context.apiUrl}/proxy/v1`, {
  targetUrl: 'https://api.openai.com/v1/chat/completions',
  proxyHeaders: JSON.stringify({
    'Content-Type': 'application/json',
    'Authorization': 'Bearer ${{var.OPENAI_API_KEY}}'
  }),
  workspaceId: context.workspace.id,
  proxyBody: JSON.stringify({
    model: 'gpt-4',
    messages: updatedMessages,
  })
}, {
  headers: {
    'Content-Type': 'application/json',
  }
});

```

In this example:

* `${{var.OPENAI_API_KEY}}` is a placeholder for a sensitive key that will be injected by the proxy.
* `proxyBody` contains the data to be sent in the request body.
* `proxyHeaders` contains the headers to be sent to the target URL.
* `targetUrl` specifies the API endpoint to be invoked.
* `workspaceId` is used for validations on the proxy side.

By using the Action Proxy, you can easily interact with external APIs while using Terrakube's capabilities to manage and inject necessary variables securely.
