In this case, there is no profile assigned—the most likely reason is that the Autopilot device object does not belong to the right group where the Autopilot profile is assigned. You can also see that this device is in Azure AD but not in Intune. There is a lot of useful information on the Autopilot device object when you click on it.

You cannot assign an Autopilot profile directly to a device from Microsoft Intune; you can only do this for an Azure AD group. From Microsoft Store for Business, you can assign a profile directly to a device. This can break the expected Windows Autopilot onboarding experience and make troubleshooting harder for yourself.

# Windows 11 Autopilot diagnostics page

When you deploy Windows 11 with Autopilot, you can enable users to view additional detailed troubleshooting information about the Autopilot provisioning process. A new Windows Autopilot diagnostics page provides IT admins and end users with a user-friendly view to troubleshoot Windows Autopilot failures.

If you set Turn on log collection and diagnostics page for end users to Yes, you will enable Windows Autopilot diagnostics on an Autopilot device, as illustrated in the following screenshot:

![](bbox=[0.27, 0.495, 0.72, 0.69])