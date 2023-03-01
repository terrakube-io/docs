# Workspace scheduler

Terrakube allows to schedule a job inside workspaces, this will allow to execute a specific [template](../organizations/templates/) in an specific time.

To create a workspace schedule click the "schedule" tab and click "Add schedule" like the following image.

<figure><img src="../../.gitbook/assets/image (2) (1) (4).png" alt=""><figcaption></figcaption></figure>

This is usefull for example when you want to destroy your infrastructure by the end of every week to save some money with your cloud provider and recreate it every monday morning.

Example.

Lets destroy the workspace every friday at 6 pm

{% hint style="info" %}
Cron expression will be 30 18 \* \* 5
{% endhint %}

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Lets create the workspace every monday at 5:30 am

{% hint style="info" %}
Cron expression will be 30 5 \* \* 1
{% endhint %}

<figure><img src="../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>
