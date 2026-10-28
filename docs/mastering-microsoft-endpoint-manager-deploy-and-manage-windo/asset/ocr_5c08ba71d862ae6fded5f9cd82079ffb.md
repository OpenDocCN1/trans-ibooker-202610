The Microsoft Intune license is a part of Microsoft 365 E3, Microsoft 365 F1/F3 for Firstline Workers, EMS E3, as well as standalone Microsoft Intune.

There are many other licenses that give you the right to Microsoft Intune. Always consult your license partner to find out the correct license for your scenario.

A user in your tenant also requires a license to enroll their device into Intune.

A recommendation for assigning your users Intune licenses is to leverage Azure AD group-based licensing.

# Azure AD group-based licensing

Microsoft paid cloud services, such as Microsoft 365, EMS, Windows 10, Office 365, Dynamics 365, and other similar products, require licenses. These licenses are assigned to each user who needs access to these services. Administrators use one of the management portals (admin.microsoft.com or aad.portal.azure.com) and PowerShell cmdlets to manage licenses. Azure AD is the underlying infrastructure that supports identity management for all Microsoft cloud services. Azure AD stores information about license assignment states for users.

Azure AD includes group-based licensing, which means one or more product licenses can be assigned to a group. Azure AD ensures that the licenses are assigned to all members of the group. This includes non-members who then become a member of the group. When they leave the group, those licenses are removed. This licensing management eliminates the need for automating license management via PowerShell to reflect changes in the organization and departmental structure on a per-user basis.

For any groups assigned a license, you must also have a license for each unique member. While you do not have to assign each member of the group a license, you must have at least enough licenses to include all the members. For example, if you have 10,000 unique members who are part of licensed groups in your tenant, you must have at least 10,000 licenses to meet the licensing assignment.

Licenses can be assigned to any security group in Azure AD. Security groups can be synced from on-premises using Azure AD Connect. You can also create security groups directly in Azure AD (also called cloud-only groups), or automatically via the Azure AD dynamic group feature. Office 365 groups cannot be used for group-based licensing.