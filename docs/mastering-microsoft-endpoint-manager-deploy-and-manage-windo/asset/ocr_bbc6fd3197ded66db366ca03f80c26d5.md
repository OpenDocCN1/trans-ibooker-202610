# Verifying identity

The majority of security breaches today involve credential theft, and lapses in cyber hygiene amplify the potential for risk to employees and to organizations at large. That's why one of the primary components of a zero trust system is the ability to verify a user's identity before access is granted to the corporate network.

Start by implementing MFA through the modern experience of Azure Authenticator and Windows Hello for Business on all Windows 10 devices.

# Verifying devices

Because unmanaged devices are an easy entry point for bad actors, ensuring that only healthy and trusted devices can access critical applications and data is vital for enterprise security. A fundamental part of our zero trust implementation is to enroll all end user devices in device management systems such as Microsoft Intune.

Enabling compliance policies to check the devices for parameters such as BitLocker encryption, secure boot, and device health verification in this way is essential to managing the policies that govern access to corporate resources.

Microsoft Endpoint Manager is a key piece of this new way of thinking as it makes the device trusted, with a compliance policy to validate whether the devices are compliant.

Conditional Access is the tool built into Azure AD to bring signals together, make decisions, and enforce organizational Conditional Access policies:

+   - **Signals**: A device or a user sign-in with a specific application or browser.
+   - **Decision**: What level of access you will be granted based on the policies.
+   - **Enforcement**: This could be as simple as giving the end user access with an MFA token or denying access:

![](bbox=[0.115, 0.68, 0.835, 0.835])

Figure 2.6 – Zero trust verifying devices diagram