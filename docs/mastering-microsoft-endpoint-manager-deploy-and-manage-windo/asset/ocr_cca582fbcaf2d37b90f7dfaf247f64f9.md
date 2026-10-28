Another great example of a device configuration template is **Shared multi-user device**, which is used to configure access, account deletion, maintenance, and power management settings for a shared Windows 10 device used by multiple users in your organization:

![](bbox=[0.135, 0.175, 0.865, 0.66])

Figure 10.40 – Shared multi-user device

End users can sign into these shared devices with an AAD account. Once they’ve signed in, their credentials are cached. As they use the device, end users only get access to features you allow. For example, you choose when the device goes into sleep mode and whether users can see and save files locally, enable or disable power management settings, and more. You also control whether the AAD account will be deleted by setting a certain threshold; the user account will be deleted when it is reached. This feature uses the same local user profile that has been optimized for shared use, so the login time for a first-time user on the device will only be a few seconds.