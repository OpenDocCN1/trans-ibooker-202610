The Contributor role has permissions to read and modify any configuration on the management plane of this service (except for assigning permissions to other users) but it does not have inherent access to read or modify data in the data plane based on RBAC permissions (Figure 6.7). This may sound confusing but note that the Contributor role policy document does not specify any dataActions permissions:

```
"permissions": [
  {
    "actions": [
      "*"
    ],
    "notActions": [
      "Microsoft.Authorization/*/Delete",
      "Microsoft.Authorization/*/Write",
      "Microsoft.Authorization/elevateAccess/Action",
      "Microsoft.Blueprint/blueprintAssignments/write",
      "Microsoft.Blueprint/blueprintAssignments/delete",
      "Microsoft.Compute/galleries/share/action"
    ],
    "dataActions": [],
    "notDataActions": []
  }
]
```

What the role can do

What the role is excluded from doing

The role has no permissions on the data plane by default

Figure 6.7 – Azure Contributor permissions

An Azure AD identity needs to be granted additional dataActions permissions such as the ones listed in Figure 6.8 to access the data plane with an Azure AD access token. As Microsoft moves more toward this pattern of exposing service data plane access through Azure RBAC, there will be more opportunities to exploit the data plane of services in Azure without having to pivot to another option:

|  | Role | Description |
|---|---|---|
| ☐ | Storage Blob Data Contributor | Allows for read, write and delete access to Azure Storage blob containers and data |
| ☐ | Storage Blob Data Owner | Allows for full access to Azure Storage blob containers and data, including assigning POSIX access cont |
| ☐ | Storage Blob Data Reader | Allows for read access to Azure Storage blob containers and data |
| ☐ | Storage File Data SMB Share Contributor | Allows for read, write, and delete access in Azure Storage file shares over SMB |
| ☐ | Storage File Data SMB Share Elevated Co... | Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB |
| ☐ | Storage File Data SMB Share Reader | Allows for read access to Azure File Share over SMB |
| ☐ | Storage Queue Data Contributor | Allows for read, write, and delete access to Azure Storage queues and queue messages |
| ☐ | Storage Queue Data Message Processor | Allows for peek, receive, and delete access to Azure Storage queue messages |
| ☐ | Storage Queue Data Message Sender | Allows for sending of Azure Storage queue messages |
| ☐ | Storage Queue Data Reader | Allows for read access to Azure Storage queues and queue messages |

Figure 6.8 – Azure Storage-related roles