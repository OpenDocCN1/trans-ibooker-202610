
- DiagnosticLogCSP_Collector_DeviceEnrollment.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_1_31_20_4_38.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_2_11_4_18_29.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_2_24_19_11_54.etl
- DiagnosticLogCSP_Collector_DeviceProvisioning_2021_3_13_3_31_33.etl
- IntuneManagementExtension-20210228-125401.log
- IntuneManagementExtension-20210319-102847.log
- IntuneManagementExtension.log
- LicensingDiag.cab
- LicensingDiag_Output.txt
- MdmDiagLogMetadata.json
- MdmDiagReport_RegistryDump.reg
- MdmLogCollectorFootPrint.txt
- microsoft-windows-aad-operational.evtx
- microsoft-windows-appxdeploymentserver-operational.evtx
- microsoft-windows-assignedaccess-admin.evtx
- microsoft-windows-assignedaccess-operational.evtx
- microsoft-windows-assignedaccessbroker-admin.evtx
- microsoft-windows-assignedaccessbroker-operational.evtx
- microsoft-windows-crypto-ncrypt-operational.evtx
- microsoft-windows-devicemanagement-enterprise-diagnostics-provider-admin.evtx
- microsoft-windows-devicemanagement-enterprise-diagnostics-provider-debug.evtx
- microsoft-windows-devicemanagement-enterprise-diagnostics-provider-operational.evtx
- microsoft-windows-moderndeployment-diagnostics-provider-autopilot.evtx
- microsoft-windows-moderndeployment-diagnostics-provider-managementservice.evtx
- microsoft-windows-provisioning-diagnostics-provider-admin.evtx
- microsoft-windows-provisioning-diagnostics-provider-autopilot.evtx
- microsoft-windows-provisioning-diagnostics-provider-managementservice.evtx
- microsoft-windows-shell-core-operational.evtx
- microsoft-windows-user device registration-admin.evtx
- Sensor-20210105-105450.log
- Sensor-20210207-191747.log
- Sensor.log
- setupact.log
- TpmHliInfo_Output.txt
- _IntuneManagementExtension.log

The most important information you are getting from the MDM diagnostics tools is as follows:

```
Registry Dump
MdmDiagReport_RegistryDump.reg captures the
HKEY_LOCAL_MACHINE
HKEY_CURRENT_USER
Registry values associated with autopilot device provisioning are written to HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Provisioning\Diagnostics\AutoPilot

HKEY_CURRENT_USER\software\microsoft\enterprisemodernappmanagement and HKEY_LOCAL_MACHINE\software\microsoft\enterprisemodernappmanagement
```

In the case of reporting inside the Microsoft Endpoint Manager admin center, the process is as follows.

In the Microsoft Endpoint Manager admin center, https://endpoint.microsoft.com/, choose Devices | Monitor | Autopilot deployments (preview).

You can see your Windows Autopilot deployments from the last 30 days:

![](img/a3e9f26159d3739e434fe676b2987c1d_265_0.png)

Figure 7.18 – Autopilot report

You can see the following attributes:

- **Enrollment date**
- **Enrollment method**
- **Serial number**
- **Device name**
- **User**
- **Autopilot profile**
- **Enrollment status page deployment state**
- **Deployment total time**

## Company Portal

Company Portal is a self-service tool for end users so that they can install apps that the IT admin has made available for end users.

For Windows 10 Autopilot-provisioned devices, it is recommended that you associate your Microsoft Store for Business account with Intune. The Company Portal app will be installed in the device context when assigned to the Autopilot group and will be installed on the device before the user logs in. You can choose to install the Company Portal (offline) app using the steps shown next:

- 1. In the Microsoft Endpoint Manager admin center, `https://endpoint.microsoft.com/`, choose **Apps | Windows**.
- 2. From the list of Windows apps, select **Company Portal** (Offline).
- 3. Assign Company Portal as a required app to your selected Autopilot device group.
- 4. As this is an offline app, be sure to change **License type** to **Device licensing**.

## Configuring automatic BitLocker encryption for Autopilot devices

With Windows Autopilot, BitLocker encryption settings can be configured to be applied before automatic encryption starts. This configuration ensures that the default encryption algorithm is not applied automatically. Other BitLocker policies can also be applied before automatic BitLocker encryption begins.

BitLocker automatic device encryption uses BitLocker drive encryption technology to automatically encrypt internal drives after the user completes the OOBE on *Modern Standby* or **Hardware Security Testability Specification (HSTI)**-compliant hardware.

In the case of Modern Standby requirements or HSTI validation, this requirement is met by one of the following:

- Modern Standby requirements are implemented. These include requirements for **Unified Extensible Firmware Interface (UEFI)** Secure Boot and protection from unauthorized **Direct Memory Access (DMA)**.
- Starting with *Windows 10 version 1703*, this requirement can be met through an HSTI test.
- The Platform Secure Boot self-test (or additional self-tests as configured in the registry) must be reported by the HSTI as implemented and passed.
- Excluding Thunderbolt, the HSTI must not report any non-allowed DMA buses.
- If Thunderbolt is present, the HSTI must report that Thunderbolt is configured securely (the security level must be SL1 – *User Authorization* or higher).

*Windows 10 (Modern Standby)* expands the *Windows 8.1 Connected Standby power model*. Connected Standby, and consequently Modern Standby, enable an instant on/instant off user experience, like smartphone power models. Just like a phone, the S0 low-power idle model enables the system to stay connected to the network while in a low-power mode.

By leveraging the `powercfg /a` command, you can see whether your device supports Modern Standby (S0 Low Power Idle):

![](img/a3e9f26159d3739e434fe676b2987c1d_267_0.png)

Figure 7.19 – powercfg

## Important Note

BitLocker automatic device encryption starts during the OOBE experience. However, BitLocker drive protection is enabled only after users sign in with an AAD account. Until then, protection is suspended, and data is not protected.

The BitLocker encryption algorithm is used when BitLocker is first enabled. The algorithm sets the strength for full volume encryption. Available encryption algorithms are AES-CBC 128-bit, AES-CBC 256-bit, XTS-AES 128-bit, and XTS-AES 256-bit encryption. BitLocker will use the default encryption method of XTS-AES 128-bit or the encryption method specified by any setup script.

To make sure the BitLocker encryption algorithm you want is set before automatic encryption occurs for Autopilot devices, make sure the following requirements are fulfilled:

- The device contains a **Trusted Platform Module (TPM)**, either TPM 1.2 or TPM 2.0.
- UEFI Secure Boot is enabled.
- Platform Secure Boot is enabled.
- DMA protection is enabled.
- Configure the **Encryption method** settings in the Windows 10 Endpoint Protection profile to the encryption algorithm you want.
- Assign the policy to your Autopilot device group. The encryption policy must be assigned to devices in the group, not users.
- Enable the Autopilot ESP for these devices. If the ESP is not enabled, the policy won't apply before encryption starts.

## Important Note

When you enable EncryptionMethodByDriveType, you must specify values for all three drives (OS, fixed data, and removable data); otherwise, it will fail (500 return status). For example, if you only set the encryption method for the OS and removable drives, you will get a 500 return status.

Create the BitLocker policy in Intune for automatic BitLocker encryption as follows:

- 1. In the Microsoft Endpoint Manager admin center, `https://endpoint.microsoft.com/`, choose **Devices | Windows | Configuration profiles**.
- 2. Choose **Create Profile**.
- 3. Select **Platform | Windows 10 and later**.
- 4. Select **Profile type | Templates | Template name | Endpoint Protection**.
- 5. Select **Name | Autopilot Bitlocker | Next**.
- 6. Expand **Windows Encryption**.
- 7. Select **Windows Settings | Encrypt devices | Require**.

![](img/a3e9f26159d3739e434fe676b2987c1d_269_0.png)

The BitLocker base settings should be as follows:

- 1. Select **BitLocker base settings | Warning for other disk encryption | Block**.

  Selecting **Block** will disable the BitLocker configuration prompt and the warning prompt for other disk encryptions. Block is needed to automatically BitLocker encrypt a device.
- 2. Select **Allow standard users to enable encryption during Azure AD Join | Allow**.

  This setting, when set to **Allow**, enables the end user to automatically encrypt the device when they are not a local admin.
- 3. For **Configure encryption methods**, select what your organization requires. The Windows default is **XTS-AES 128-bit**, which is also the recommended value in the Windows security baseline:

![](img/a3e9f26159d3739e434fe676b2987c1d_269_1.png)
