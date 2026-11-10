# Template Scheduling in Jobs

When we deploy some infrastructure we can set some other template to execute in some specific time for example to destroy the infrascturture or resize it.

{% hint style="success" %}
This example will show the basic logic to accomplish but it is not creaing a real VM it just  running some scripts that are showing the injected  terraform variables when running the job.
{% endhint %}

### Example

Define the size as global terraform variables for example SIZE\_SMALL and SIZE\_BIG, this terraform variables will be injected in our future templates "schedule1" and "schedule2" as terraform variables dynamically.\


<figure><img src="https://user-images.githubusercontent.com/4461895/225761283-27d0a9c2-6c3d-430c-9d80-66cb6a2d51d8.png" alt=""><figcaption></figcaption></figure>

We define a job template that creates the infrasctucture using some small size and create two schedule at the end of the template.

### Create Virtual Machine (Example)

```
flow:
  - type: "customScripts"
    step: 100
    inputsTerraform:
      SIZE: $SIZE_SMALL
    commands:
      - runtime: "BASH"
        priority: 100
        before: true
        script: |
          echo "SIZE : $SIZE"
  - type: "scheduleTemplates"
    step: 200
    templates:
      - name: "schedule1"
        schedule: "0 0/5 * ? * * *"
      - name: "schedule2"
        schedule: "0 0/10 * ? * * *"
```

{% hint style="info" %}
This will create our "virtual machine" using size small, after it will set two addtional schedules to run every 5 and 10 minutes to resize it.
{% endhint %}

We will have 3 templates, one to create the virtual machine and two to resize it

<figure><img src="https://user-images.githubusercontent.com/4461895/225761347-18b09e5d-29ae-4815-b27f-ea219db8ffa8.png" alt=""><figcaption></figcaption></figure>

Once the workspace is executed creating the infrasctructure two automatic shedules will be define

<figure><img src="https://user-images.githubusercontent.com/4461895/225761484-ad328fb4-7275-48b0-ab99-3a6a7a219488.png" alt=""><figcaption></figcaption></figure>

Differente jobs will be executed.

<figure><img src="../../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

### Resize Template (schedule1)

Inject the new size using global terraform variables

```
flow:
  - type: "customScripts"
    step: 100
    inputsTerraform:
      SIZE: $SIZE_SMALL
    commands:
      - runtime: "BASH"
        priority: 100
        before: true
        script: |
          echo "SIZE : $SIZE"
```

<figure><img src="https://user-images.githubusercontent.com/4461895/225762481-af2a0779-8383-472a-90a1-61fd362d0c50.png" alt=""><figcaption></figcaption></figure>

### Resize Template (schedule2)

Inject the new size using global terraform variables

```
flow:
  - type: "customScripts"
    step: 100
    inputsTerraform:
      SIZE: $SIZE_BIG
    commands:
      - runtime: "BASH"
        priority: 100
        before: true
        script: |
          echo "SIZE : $SIZE"
```

<figure><img src="https://user-images.githubusercontent.com/4461895/225762637-f18f0335-c49a-4761-9a25-446ae196f8d1.png" alt=""><figcaption></figcaption></figure>

