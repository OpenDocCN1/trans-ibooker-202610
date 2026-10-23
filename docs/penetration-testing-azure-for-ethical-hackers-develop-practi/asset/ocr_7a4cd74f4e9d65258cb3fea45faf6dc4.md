# Stealing credentials from a system

Depending on what your scope is, you may find yourself in an internal network environment or on an Azure VM, looking to persist in the tenant. While we could easily make an argument to escalate your privileges to a domain administrator account and compromise the AD sync server, that may not always be a practical option.

For a stealthier option, you may want to look at utilizing the system of an existing Azure user. Once you are on an Azure user's system, you have a couple of options.

If you have access to the actual user account, you may be able to make use of an existing Azure portal session in the web browser. Alternatively, the system may also be configured to use passthrough single sign-on (SSO), so opening https://portal.azure.com in Internet Explorer as the user could let you right in.

> **Important Note**
> The technique outlined here has been covered on the Lares blog by the following post, written by *Lee Kagan*: https://www.lares.com/blog/hunting-azure-admins-for-vertical-escalation/.
> Please refer to the post for additional information on this specific attack.

If the user has some account restrictions, such as multi-factor authentication or conditional access policies, your best bet is to export any existing authentication tokens from the Az CLI and/or the Az PowerShell cmdlets. These tokens are typically stored in the c:\Users\%USERNAME%\.Azure directory on a Windows operating system client. Keep in mind that the users that you are attacking may be clearing up their cached credentials on their systems, so it's not a guarantee that a specific user will have active Azure credentials stored on their system.

# Exporting PS/AZ credentials from an Admin system

If you do not have access to the clear-text user account credentials, you can still look for existing active tokens in the .Azure folder. This will most likely require you to have local administrator access to the system, so keep that in mind: