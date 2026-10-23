# FIGURE 2.16 A list of users in Azure Active Directory

![](bbox=[0.13, 0.12, 0.85, 0.28])

# Self-Service Password

Changing, resetting, and unlocking accounts created a lot of work for support teams before the invention of global identity repositories like Active Directory. The in-house development teams quickly got this feedback and built self-service features for changing, resetting, and unlocking passwords into their application.

With the Free edition, the only supported capability is the ability to change a password. If the user is mostly in Azure, this is done here: myapps.microsoft.com. In some standard use-case scenarios, a custom security policy can provide a trigger that requires users to change their password every 30 to 90 days.

The functionality to reset and unlock accounts is available with the Free edition; however, it is self-help—back to the old days. In the Basic and Premium editions, there are built-in self-service capabilities for resetting and unlocking passwords. This is an optimal place to perform cost analysis. In other words, take the number of accounts you have and multiply that by the cost per user, and include the probability of how often users would need to unlock or reset their password. Is that amount greater than or equal to the cost of running Azure Active Directory in Basic or Premium?

# Application Proxy

This feature reminds me a bit of the Hybrid Connection Manager (HCM), which will be discussed in the next chapter, where we hash out Azure networking. The similarity comes from the fact that there is a connector service, aka connection manager, that is downloaded from the Azure Portal and is typically installed on a server within an on-premise customer network. Navigating to the Azure Active Directory blade and clicking the Azure Proxy link renders a link for downloading the connector, as shown in Figure 2.17.

# FIGURE 2.17 Application Proxy link

![](bbox=[0.42, 0.75, 0.57, 0.78])