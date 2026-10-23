Here is a sample output of this command:

![](bbox=[0.12, 0.14, 0.86, 0.32])

Figure 7.3 – PowerShell command results for sign-in auditing

The following Azure AD built-in roles have the permissions needed to read this information: Security Administrator, Security Reader, and Report Reader. Additionally, any custom Azure AD role that has the following permissions could also be used:

+   - microsoft.directory/groups/allProperties/allTasks - Grants permission to read and update group properties in Azure AD
+   - microsoft.directory/signInReports/allProperties/read - Grants permission to read sign-in reports

It is also worth noting that there are opportunities to move laterally from Azure AD to other cloud applications that may be using Azure AD as their identity provider if your pentest scope covers that. You can discover other applications that have a trust relationship with Azure AD by reviewing the Enterprise Applications in Azure AD. You can use this section to find out the users authorized for the applications and the groups that they belong to. If the access is granted via groups, you can add yourself to the groups to gain permissions to those cloud applications.

# Path 2 – Resetting user passwords

Another path that could be exploited to move laterally to another Azure subscription is to reset the password of a user who has permissions to another Azure subscription and logging in as that user. This path is less preferred as it impacts the existing access of a user and is more intrusive. It easily raises an alarm if a user is suddenly not able to log in to their account with the password that they have always used, and the audit logs reflect the recent password change event.