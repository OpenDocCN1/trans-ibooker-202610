+   - Append
+   - Audit
+   - AuditIfNotExists
+   - Deny
+   - DeployIfNotExists
+   - Disabled
+   - Modify

You can read about their usage in the article Understand Azure Policy effects.

# Creating Your Own Azure Policy

Creating your own Azure policy is easy. In the following example, we are creating a policy that only allows the deployment of resources in Azure regions within the United States. If a user attempts to create a resource in a non-U.S. region, it will be denied:

```
{
  "properties": {
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "mode": "Indexed",
    "metadata": {
      "version": "1.0.0",
      "category": "Locations"
    },
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    }
  }
}
```