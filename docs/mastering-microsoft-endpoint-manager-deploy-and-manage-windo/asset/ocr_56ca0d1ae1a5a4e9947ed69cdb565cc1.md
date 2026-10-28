# Hybrid AAD

Devices that are HAADI are part of your organization and are signed in with an AD (AD DS) account belonging to the organization. They exist in the cloud and on-premises to make the translation possible between traditional AD DS and AAD. This is needed for different scenarios within MEM as well as *Windows 365*.

To use hybrid AAD, you must replicate your on-premises environment to bring/sync your identities and devices to AAD. You do that with the Microsoft AAD Connect software.

The software has been built on **Microsoft Identity Management (MIM)** and is preconfigured with all the configuration items to replicate your on-premises users, groups, and devices to AAD.

![](bbox=[0.133, 0.329, 0.866, 0.628])

*Figure 13.8 – Microsoft AAD Connect – Express Settings*

If you have configured AAD Connect in the past, you must change the configuration to HAADJ for services such as Windows 365. Hybrid AAD could also be beneficial in the following alternative scenarios:

+   - You have Win32 apps deployed to these devices that rely on AD machine authentication.
+   - You want to continue to use Group Policy to manage device configuration.
+   - You want to continue to use existing imaging solutions to deploy and configure devices.
+   - You must support down-level Windows 7 and 8.1 devices in addition to Windows 13.