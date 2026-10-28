
# Signing in to Graph Explorer

Enter `https://graph.microsoft.com/v1.0/devices` to get all the devices – then you can find the device you just created and see that it has the ZTDID, with a unique value, and the Group Tag is shown as OrderID in Graph. You can also run the graph call with the ID at the end and only get the value for a single device: `https://graph.microsoft.com/v1.0/devices/[id]`. The data is stored in a multi-value attribute called physicalIds:

![](img/a3e9f26159d3739e434fe676b2987c1d_258_0.png)

*Figure 7.15 – Graph Explorer devices*

Then, you can find `physicalIds` as an attribute on the device object in the graph, to verify that there is a ZTDID on the AzureAD device object – ZTDID means it is an Autopilot object. You can also see OrderID (OrderID is the same as Group Tag in the Microsoft Endpoint Manager admin center):

```
```json
"physicalIds": [
    "[HWID]:h:6755414090630361",
    "[ZTDID]:able4d57-66e5-4143-bb43-753be871075f"
]
```
```

```
" [OrderId] :EdgeKIOSK",
" [USER-HWID] :44cd8da3-8f37-49e0-aa01-93c7179969d1:6755414090630361",
" [GID] :g:6825777827713522",
" [USER-GID] :44cd8da3-8f37-49e0-aa01-93c7179969d1:6825777827713522",
],
```

Now we have all the information we need to create two dynamic AAD groups, one for all Autopilot devices and one for EdgeKIOSK.

We will create three dynamic groups so we can distinguish between the different Windows Autopilot scenarios. We are naming the groups All AutoPilot Devices, All AutoPilot EdgeKIOSK, and All AutoPilot JSON.

In the Microsoft Endpoint Manager admin center, https://endpoint.microsoft.com/, choose Groups.

Choose New Group.

Enter the following in the Group name field: All Autopilot Devices.

Choose Dynamic Device for Membership type:

![](img/a3e9f26159d3739e434fe676b2987c1d_259_0.png)

Figure 7.16 – New AAD device group

The first group, **All AutoPilot Devices**, has a dynamic group membership rule such as this:

```
```text
(device.devicePhysicalIDs -any _ -contains "[ZTDId]") -and -not
(device.devicePhysicalIDs -any _ -eq "[OrderID]: EdgeKIOSK")
```
```

This rule will find all devices with the ZTDID and exclude all devices where the value of OrderID is EdgeKIOSK:

![](img/a3e9f26159d3739e434fe676b2987c1d_260_0.png)

*Figure 7.17 – AAD group Autopilot rule syntax*

The second group, **All AutoPilot EdgeKIOSK**, has a dynamic group membership rule such as this: `(device.devicePhysicalIDs -any _ -eq "[OrderID]: EdgeKIOSK")`.

This rule will find all devices where the value of OrderID is equal to EdgeKIOSK.

You can also just create the AAD dynamic groups with PowerShell:

```
```powershell
Install-Module AzureAD
$AzureAdCred = Get-Credential
Connect-AzureAD -Credential $AzureAdCred
# Create a dynamic group called "All AutoPilot Devices"
New-AzureADMSGroup -Description "All AutoPilot Devices"
-DisplayName "All AutoPilot Devices" -MailEnabled $false
-SecurityEnabled $true -MailNickname "Win" -GroupTypes
"DynamicMembership" -MembershipRule "(device.devicePhysicalIDs
-any _ -contains \"[ZTDId]\") -and -not (device.
devicePhysicalIDs -any _ -eq \"[OrderID]:SharedDevice\")"
-MembershipRuleProcessingState "On"
# Create a dynamic group called "All AutoPilot EdgeKIOSK"
```
```

```
New-AzureADMSGroup -Description "All AutoPilot EdgeKIOSK" -DisplayName "All AutoPilot EdgeKIOSK" -MailEnabled $false -SecurityEnabled $true -MailNickname "Win" -GroupTypes "DynamicMembership" -MembershipRule "(device.devicePhysicalIds -any _ -eq "[OrderID]:EdgeKIOSK")" -MembershipRuleProcessingState "On"
```

```
# Create a dynamic group called "All AutoPilot JSON"
```

```
New-AzureADMSGroup -Description "All AutoPilot JSON" -DisplayName "All AutoPilot JSON" -MailEnabled $false -SecurityEnabled $true -MailNickname "Win" -GroupTypes "DynamicMembership" -MembershipRule "(device.enrollmentProfileName -eq "OfflineAutopilotprofile-264c05b0-683c-4537-87ff-1ff5151d5b98")" -MembershipRuleProcessingState "On"
```

Now you have imported the hardware information into Windows Autopilot and created all the groups you need to process by configuring the enrollment status page.

# Enrollment Status Page (ESP)

Many think that ESP and Windows Autopilot are one and the same. That is not necessarily the whole truth as ESP is a core Windows function and can be set as a Windows CSP.

ESP can be used as part of any Windows Autopilot provisioning scenario. It can also be used separately from Windows Autopilot as part of the default OOBE for AAD join, as well as for any new users signing in to the device for the first time.

Basically, what that means is that any Windows 10 devices that join AAD in the OOBE phase will have the ESP shown to the end user, irrespective of whether it is a Windows Autopilot device, as long as there is an ESP assigned to the user or device.

You can create multiple ESP profiles with different configurations that specify the following:

- Showing the installation progress
- Blocking access until the provisioning process is complete
- Time limits
- Allowed troubleshooting operations

We will show you how to do this in this section and, in a later section, with a Windows Autopilot kiosk scenario.

# ESP implementation Windows CSP

ESP uses the `EnrollmentStatusTracking` **Configuration Service Provider (CSP)** and the `FirstSyncStatus` CSP to track the installation of different apps:

- The `EnrollmentStatusTracking` CSP: `/Vendor/MSFT/EnrollmentStatusTracking`.
- The `EnrollmentStatusTracking` CSP: Windows Client Management | Microsoft Docs (`https://docs.microsoft.com/en-us/windows/client-management/mdm/enrollmentstatustracking-csp`).
- The `EnrollmentStatusTracking` CSP is supported in Windows 10 version 1903 and later.
- The `EnrollmentStatusTracking` CSP is used to track the installation of the Microsoft Intune Management extension and Microsoft Intune Win32 apps that are target devices and/or users that are blocked by the ESP.
- The `FirstSyncStatus` CSP: `/Vendor/MSFT/DMClient/Provider/ProviderID/FirstSyncStatus`.
- The DMClient CSP – Windows Client Management | Microsoft Docs (`https://docs.microsoft.com/en-us/windows/client-management/mdm/dmclient-csp`).
- The `FirstSyncStatus` CSP is supported in Windows 10 version 1709 and later.
- The `FirstSyncStatus` CSP is responsible for delivering the ESP CSP payload from Microsoft Intune to the Windows 10 client. The payload includes ESP settings such as the timeout period and applications that are required to be installed. It also delivers the expected MSI (line-of-business) applications, Microsoft Store for Business apps, Wi-Fi profiles, and SCEP certificate profiles, as well as policies for Microsoft Edge, assigned access, and Kiosk Browser.

We will look at the steps to create an ESP as follows:

1. Create the first ESP by starting the Microsoft Endpoint Manager admin center, `https://endpoint.microsoft.com/`, and choose **Devices | Windows | Windows enrollment | Enrollment Status Page**.
2. Choose a profile under **Settings**.
3. Choose **Yes** for **Show app and profile installation progress**.
4. Choose **Yes** for **Block device use until all apps and profiles are installed**.
5. Choose **Selected** for **Block device use until these required apps are installed if they're assigned to the user/device**.
6. Choose **Select apps**, and then select **Select | Save** for the apps.

In the **Settings** step, configure the following values and then click **Next**:

| Setting | Value |
|---|---|
| Show app and profile configuration process | Yes |
| Show an error when installation takes longer than the specified number of minutes | 60 |
| Show custom message when time limit error occurs | Yes (you may modify the default message) |
| Allow users to collect logs about installation errors | Yes |
| Only show page to devices provisioned by out-of-box experience (OOBE) | Yes |
| Block device user until all apps and profiles are installed | Yes |
| Allow users to reset the device if an installation error occurs | Yes |
| Allow users to use the device if an installation error occurs | No |
| Block device user until these required apps are installed if they are assigned to the user/device | All or select up to 100 apps |

Now that we have seen how to create an enrollment page, let's move on and learn about Autopilot reporting and diagnostics.

# Autopilot reporting and diagnostics

Run cmd.exe as an administrator, and then you can run MdmDiagnosticsTool to get logs from the local device:

```
C:\windows\system32\MdmDiagnosticsTool.exe -area Autopilot -cab
C:\temp\MdmDiagnostics.cab
```

You will then get the MdmDiagnostics.cab file, where you can extract the content to a folder to get access to the content:

```
AgentExecutor-20210331-083241.log
AgentExecutor.log
ClientHealth.log
DeviceHash_DESKTOP-86N3?3H3.csv
DiagnosticLogCSP_Collector_Autopilot.etl
```
