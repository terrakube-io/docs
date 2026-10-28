# Deprecating and Removing Versions

{% hint style="info" %}
**Manage Modules** permission is required to change the status of a module version, and **Manage Providers** permission for a provider version. Please check [team-management.md](../organizations/team-management.md "mention") for more info.
{% endhint %}

Every module and provider version in the private registry has one of three statuses:

| Status         | Served by the registry | What users see                                                                                   |
| -------------- | ---------------------- | ------------------------------------------------------------------------------------------------ |
| **Active**     | Yes                    | Nothing special. New versions are active.                                                        |
| **Deprecated** | Yes                    | A warning on the registry page, with your message. For providers, `terraform init` also warns.   |
| **Removed**    | No                     | The version is hidden from the version list and cannot be downloaded. `terraform init` fails for anyone who requires it. |

Use **Deprecated** to announce that a version is going away, for example with a removal date or upgrade instructions. Use **Removed** to stop a broken release from being used.

Both are reversible: set the version back to **Active** at any time.

### Changing the status of a version

1. Open the module or provider in the **Registry** and select the version.
2. In the **Manage** menu, click **Change status of version X…**.
3. Choose **Active**, **Deprecated** or **Removed**, and optionally add a message (up to 1024 characters).
4. Click **Save**. When you choose **Removed**, the button reads **Remove version X**.

On the registry page:

- the version picker labels deprecated and removed versions and lists removed versions last;
- a deprecated or removed version shows a banner with your message, or suggests the newest active version when there is no message;
- the page opens on the newest version that is still served.

### What Terraform sees

**Modules.** The module registry protocol has no deprecation field, so a deprecated module version works as before and the deprecation is only shown on the registry page.

**Providers.** When a provider has deprecated versions, `terraform init` prints one warning for everyone who uses the provider, whichever version they use:

```
Warning: Additional provider information from registry

The remote registry returned warnings for terrakube.example.com/myorg/random:
- Versions 3.0.0, 3.0.1 of myorg/random are deprecated. See the private registry for details.
```

The warning lists at most five versions. Your messages are not printed by Terraform; they are shown on the registry page.

### Removing a version

{% hint style="warning" %}
Removing a version makes `terraform init` fail for every configuration that requires exactly that version and, for providers, for every dependency lock file that selected it. Configurations with a version constraint that allows other versions pick the newest version that is still served.
{% endhint %}

- A removed module version stops being downloadable within 30 seconds on every registry replica. Each replica also caches the module's version list (`ModuleVersionsCacheTtlSeconds`, 10 minutes by default). If you remove the newest version, one run on a replica can still pick it from that list and fail, but the replica then refreshes its list, so the next run picks the newest version that is still served.
- A removed provider version is no longer served as soon as it is removed.
- Removed versions stay in the database. The module refresh job does not import them again, and the module's latest version moves to the newest version that is still served.

### Using the API

The status is the `status` attribute (`active`, `deprecated` or `removed`) of a module version or provider version, and the message is `deprecationMessage`. Control characters are removed from the message when it is saved.

Deprecate a module version:

```
PATCH https://terrakube-api.example.com/api/v1/organization/{organizationId}/module/{moduleId}/version/{versionId}
Content-Type: application/vnd.api+json
Authorization: Bearer (PAT TOKEN)
{
    "data": {
        "type": "module_version",
        "id": "{versionId}",
        "attributes": {
            "status": "deprecated",
            "deprecationMessage": "Supported until 2026-12-31. Upgrade to 2.x."
        }
    }
}
```

Remove a provider version:

```
PATCH https://terrakube-api.example.com/api/v1/organization/{organizationId}/provider/{providerId}/version/{versionId}
Content-Type: application/vnd.api+json
Authorization: Bearer (PAT TOKEN)
{
    "data": {
        "type": "version",
        "id": "{versionId}",
        "attributes": {
            "status": "removed",
            "deprecationMessage": "Crashes on plan with S3 backends. Use 5.2.0."
        }
    }
}
```

Versions can be filtered by status, for example `GET .../module/{moduleId}/version?filter[module_version]=status==deprecated` or `GET .../provider/{providerId}/version?filter[version]=status!=removed`.
