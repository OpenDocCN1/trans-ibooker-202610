# What does WUfB allow me to configure?

When you configure WUfB policy from Microsoft Intune with the configuration setting, WUfB will be enabled. If you have traditionally utilized WSUS for servicing, your clients will connect to both Windows Update in the cloud as well as your WSUS server once you set WUfB settings. If you want to change this behavior, you need to remove your old configuration settings for WSUS on the endpoints.

Through the Windows Update Client settings (through Group Policy, MDM, or the UX), WUfB provides the ability to delay the application of updates. Quality updates and feature updates are treated separately to give you added flexibility.

In addition, you may leverage the "Pause" functionality if you need time to implement remediation to updates after testing them in earlier rings:

+   - **Feature updates**: Previously referred to as **upgrades**, feature updates contain not only security and quality revisions but also significant feature additions and changes. Feature updates are released semi-annually in the fall and the spring.
+   - **Quality updates**: Quality updates are traditional OS updates, typically released on the second Tuesday of each month (though they can be released at any time). These include security, critical, and driver updates. WUfB also treats non-Windows updates (such as updates for Microsoft Office or Visual Studio) as quality updates. These non-Windows updates are known as **Microsoft updates**, and you can set devices to receive such updates (or not) along with their Windows updates.
+   - **Driver updates**: Updates for non-Microsoft drivers that are relevant to your devices. Driver updates are on by default, but you can use WUfB policies to turn them off if you prefer.
+   - **Microsoft product updates**: Updates for other Microsoft products, such as versions of Office that are installed by using Windows Installer (MSI). Versions of Office that are installed by using Click-to-Run can't be updated by using WUfB. Product updates are off by default. You can turn them on by using WUfB policies.

Windows 10 feature updates policies work in conjunction with your *Windows 10 update ring* policies to prevent a device from receiving a Windows feature version that's later than the value specified in the feature updates policy.

For Windows 10 feature updates policies to be working, Windows 10, version 1709, or later is required. The devices need to be enrolled in Intune MDM and be hybrid AD-joined, Azure AD-joined, or Azure AD-registered. Windows telemetry needs to be turned on, with a minimum setting of Basic. If telemetry is off, Windows can be upgraded to a later version of Windows than defined in the feature updates policy.