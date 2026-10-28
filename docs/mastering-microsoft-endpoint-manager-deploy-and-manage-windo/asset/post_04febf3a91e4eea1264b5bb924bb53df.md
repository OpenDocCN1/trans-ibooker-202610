
5. Enable the settings for (at least) sign-in and strong authentication.

![](img/a3e9f26159d3739e434fe676b2987c1d_491_0.png)

You can also use a key restriction policy to specify what FIDO2 keys your end users can leverage in your tenant, by entering an allow or block list of devices with an Authenticator Attestation GUID (AAGUID).

The FIDO2 specification requires each security key provider to provide an AAGUID during attestation. An AAGUID is a 128-bit identifier indicating the key type, such as the make and model.

You’re now done with the prerequisites and are ready to use FIDO2 keys. You should see the following log-on screen for AAD while using password-less authentication via the Microsoft Authenticator mobile application, and all without entering the password.

![](img/a3e9f26159d3739e434fe676b2987c1d_491_1.png)

Password-less sign-in is more secure and the end user rarely has to enter their password anywhere.

Next, we will show a table of where the end user can leverage password-less sign-in.

## What is and isn't supported in each password-less scenario

Microsoft's password-less authentication methods enable different scenarios. The organizational needs, prerequisites, and capabilities of each authentication method need to be considered before selecting your password-less authentication strategy.

> **Note**
> There is no additional cost for password-less authentication.

In the following table, you can see the options based on different scenarios:

| Scenario | Phone authentication | Security keys | Windows Hello for Business |
|---|---|---|---|
| Computer sign-in: From an assigned Windows 10 device | No | Yes With biometric recognition and/or PIN | Yes With biometric recognition and or PIN |
| Computer sign-in: From a shared Windows 10 device | No | Yes With biometric recognition and/or PIN | No |
| Web app sign-in: From a user-dedicated computer | Yes | Yes Provided SSO to apps is enabled by computer sign-in | Yes Provided SSO to apps is enabled by computer sign-in |
| Web sign-in: From a mobile or non-Windows device | Yes | No | No |
| Computer sign-in: From a non-Windows computer | No | No | No |

This concludes the section about password-less sign-on. Next, we will cover BitLocker disk encryption.

## BitLocker disk encryption

BitLocker has been available since the first release of Windows Vista and gives the option to encrypt the drives attached to the endpoint. In most cases, BitLocker can work in conjunction with your endpoint that has a Trusted Platform Module (TPM) chip.

As long as you can authenticate to your device and you are not moving the OS disk out of the endpoint and exchanging it for another device, it's unlikely that you will ever need the BitLocker key that is associated with your device disk to decrypt everything. The help desk operator role will be able to access all the keys to restore:

- 1. To enable BitLocker for your Windows 10 or Windows 11 endpoints, you have to go to Endpoint security, followed by Disk encryption.

![](img/a3e9f26159d3739e434fe676b2987c1d_493_0.png)

Figure 13.38 – Endpoint security – Disk encryption

> Note
> BitLocker is not supported on Windows 365 as of yet.

2. Click on Create Policy.
3. Select Windows 10 and later as the platform with BitLocker for Profile.

![](img/a3e9f26159d3739e434fe676b2987c1d_494_0.png)

Figure 13.39 – BitLocker profile

4. Enter a name for the policy.
5. Change the BitLocker base settings to align them with your needs. Here's a rundown of what every setting means:

- Enable full disk encryption for OS and fixed data drives: If not configured, no BitLocker enforcement will take place. If the drive was encrypted before this policy was applied, no extra action will be taken. If the encryption method and options match that of this policy, the configuration should return success.
- BitLocker fixed drive policy: This policy setting is used to control the BitLocker for the fixed drive. The values of this policy determine the strength of the cipher that BitLocker uses for encryption.
- BitLocker system drive policy: This policy setting is used to control BitLocker for the system drive. The values of this policy determine the strength of the cipher that BitLocker uses for encryption.
- BitLocker removable drive policy: This policy setting is used to control BitLocker for the removal drive. The values of this policy determine the strength of the cipher that BitLocker uses for encryption.

Most likely, you want to enable full disk encryption for the OS and fixed drives, including potentially the rotation of the recovery password to increase security.

Consider the following best practices when configuring silent encryption on a Windows 10 device:

- Creating the BitLocker policy with the *recommended* settings.

![](img/a3e9f26159d3739e434fe676b2987c1d_495_0.png)

Figure 13.40 – Configuration settings

- In the **Fixed Drive Settings** and **OS Drive Settings** sections, you can adjust the base settings to make them stricter.

![](img/a3e9f26159d3739e434fe676b2987c1d_496_0.png)

Figure 13.41 – BitLocker fixed drive settings

> **Important Note**
> 
> BitLocker cannot silently encrypt the device if these settings are configured to required because these settings require user interaction.

After you are done with the BitLocker-specific configuration, you have to finish the assignment, most likely based on AAD users or groups.

## BitLocker recovery keys

When a problem happens with your endpoint and you need to recover your drives, you most likely need your recovery key. Luckily, the BitLocker keys are automatically saved to MEM.

You can find the device's BitLocker recovery keys under **Devices | _the user’s devices_ | Recovery keys** in MEM:

![](img/a3e9f26159d3739e434fe676b2987c1d_497_0.png)

Figure 13.42 – BitLocker recovery keys

Enter the recovery key in the key field of Windows 10 and you are good to go!

![](img/a3e9f26159d3739e434fe676b2987c1d_498_0.png)

Figure 13.43 – BitLocker recovery

You can search the device’s BitLocker recovery keys under **Devices | BitLocker keys** without knowing the device name, in the AAD admin center.

![](img/a3e9f26159d3739e434fe676b2987c1d_498_1.png)

Figure 13.44 – Searching for BitLocker keys

This concludes the section on BitLocker management in Microsoft Intune. Next, we will cover Microsoft Defender for Endpoint.

## Microsoft Defender for Endpoint

Microsoft Defender for Endpoint is Microsoft's Enterprise endpoint security platform that was created to help businesses prevent, investigate, detect, and respond to threats. This serves to increase the level of security of your whole endpoint configuration.

Microsoft Defender for Endpoint is a security solution that includes risk-based vulnerability management and assessment, attack surface reduction, behavioral-based and cloud-powered next-generation protection, **endpoint detection and response (EDR)**, automatic investigation and remediation, managed hunting services, rich APIs, and unified security management.

### Integration with MEM

MEM becomes more and more prominent for customers who are using Windows 365/Azure Virtual Desktop as it provides a unified way of configuring and maintaining your physical and virtual cloud endpoint as well as other devices such as mobile.

Microsoft Defender for Endpoint integrates seamlessly into MEM. You only need to activate the Intune integration once during the initial setup and your reports will flow into MEM.

![](img/a3e9f26159d3739e434fe676b2987c1d_499_0.png)

Figure 13.45 – Microsoft Defender for Endpoint

This concludes the overview on Microsoft Defender for Endpoint. Next, we will give you an overview of the security baselines.

## Security baselines

Security baselines are preconfigured groups of Windows settings that help you apply the security settings that are recommended by the relevant security teams. The baselines you deploy can be customized to enforce only the settings and values required by you.

There are multiple security-related settings in Windows as well as for Microsoft Edge for your endpoints. Another great asset is the option to do versioning and filtering based on different OSes or scenarios that have to be stricter. You no longer have to use GPOs to ensure the security settings on your endpoints – just create a security baseline profile and you're all set.

![](img/a3e9f26159d3739e434fe676b2987c1d_500_0.png)

Figure 13.46 – MDM security baselines

This concludes this security baseline overview. Next, we will cover compliance policies.
