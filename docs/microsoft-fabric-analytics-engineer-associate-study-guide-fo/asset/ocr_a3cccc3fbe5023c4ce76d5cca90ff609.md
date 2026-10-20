| Permission granted while sharing | Effects |
|---|---|
| Subscribe to OneLake events | The recipient can subscribe to OneLake events for lakehouses, data warehouses, mirrored databases, SQL databases, and KQL databases. |
| Build | The recipient can build new content on the semantic model. |
| Execute | The recipient can execute or cancel execution of the item. |

In summary, item-level access control is a powerful tool for implementing a scalable, secure, and manageable data environment. By combining it with the broader workspace roles, you can effectively govern who sees what in your Microsoft Fabric tenant.

However, it also adds complexity to the entire setup. You can configure explicit access to an item in a workspace without the granted user having access to the workspace. Imagine an enterprise setup with these access controls configured and you can quickly envision the effects on governance and maintenance of the setup.

# Controlling Access to Compute

Access to workspaces and specific items in one thing, but you can also allow specific users access to the underlying compute, meaning the capacity units of Fabric.