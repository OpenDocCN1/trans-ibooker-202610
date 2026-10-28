Safeguard holds prevent a device with a known compatibility issue from being offered a new OS version. The offering will proceed once a fix is issued and is verified on a held device.

The safeguard holds protection is provided by default to all devices trying to update to a new Windows 10 feature update version via Windows Update.

IT admins can, if necessary, opt devices out of safeguard protections using group policy settings or via the **Disable safeguards for Feature Updates** MDM settings.

The MDM policy can be configured as a custom policy from Microsoft Intune.
More details on how to create a custom policy in Microsoft Intune will be covered in *Chapter 10, Advanced Policy Management*. When you are creating the custom policy in Microsoft Intune, you need to fill out every mandatory field like in this example:

![](bbox=[0.21, 0.36, 0.795, 0.615])

More details are as follows:

+   - **Name:** `DisableWUfBSafeguards`
+   - **Description:** `This policy will disable safeguards`
+   - **OMA-URI:** `./Vendor/MSFT/Policy/Config/Update/DisableWUfBSafeguards`
+   - **Data type:** Integer
+   - **Value:** 1