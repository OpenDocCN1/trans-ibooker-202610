# THE FIRST ADMINISTRATOR OF THE WORKSPACE

Anyone who creates a workspace through the Fabric UI on the web experience will be the first administrator, and they can add other people or groups to the roles after creating the workspace creation.

If you are using the API for Fabric to create the workspace, the process can be performed without the creator being the first administrator. This is out of scope for this exam, though.

Workspace access control is limited to four roles and easy to maintain and develop. The additional option of item-level access control adds complexity, and we’ll get into that in the next section.

# Implementing Item-Level Access Controls

While workspace-level access control is foundational, it often proves too coarse-grained for enterprise-level security. The core principle of a robust security architecture is the principle of *least privilege*, which dictates that users should only be granted the minimum permissions they need to perform their tasks.

*Item-level access control* in Microsoft Fabric is the primary mechanism for enforcing this principle. It allows for precise, surgical management of permissions on individual items within a workspace. It represents a significant step beyond the predefined workspace roles (administrator, member, contributor, and viewer), and it provides you with the flexibility to build a highly secure and compliant data environment.