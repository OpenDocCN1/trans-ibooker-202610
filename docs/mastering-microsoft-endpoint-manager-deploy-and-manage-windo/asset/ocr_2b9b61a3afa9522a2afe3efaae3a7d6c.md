# Enforcing compliance deadlines for updates

Deploying feature or quality updates for many organizations is only part of the equation for managing their device ecosystem. The next important part is the ability to enforce update compliance. Windows Update for Business provides controls to manage deadlines for when devices should migrate to newer versions.

# How to handle conflicting or legacy policies

We sometimes find that administrators set devices to get both Group Policy settings and MDM settings from an MDM server such as Microsoft Intune. Depending on how they are ultimately set up, policy conflicts are handled differently:

+   - **Windows updates**: Group Policy settings take precedence over MDM.
+   - **Microsoft Intune**: If you set different values for the same policy on two different groups, you will receive an alert and neither policy will be set until the conflict is resolved.

It is crucial that you disable conflicting policies for devices in your organization to update as expected. For example, if a device is not reacting to your MDM policy changes, check to see whether a similar policy is set via Group Policy with a differing value.

If you find that the velocity is not as high as you expect or some devices are slower than others, it may be time to clear all policies and settings and specify only the recommended update policies outlined in this document.

# How to set up and configure Windows Update for Business

Microsoft Intune provides two policy types to manage Windows Update for Business:

+   - **Windows 10 update ring**: This policy is a collection of settings that configures when Windows 10 updates get installed. Windows 10 update ring policies have been supported since Windows 10 version 1607, so it is not something completely new.
+   - **Windows 10 feature updates**: This policy updates devices to the Windows version you specify, and then freezes the feature set version on those devices. This version freeze remains in place until you choose to update them to a later Windows version. While the feature version remains static, devices can continue to install quality and security updates that are available for their feature version. Windows 10 feature update policies are supported for devices that run Windows 10 version 1709 or later.