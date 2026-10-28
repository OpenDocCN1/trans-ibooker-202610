The first group, **All AutoPilot Devices**, has a dynamic group membership rule such as this:

```
```text
(device.devicePhysicalIDs -any _ -contains "[ZTDId]") -and -not
(device.devicePhysicalIDs -any _ -eq "[OrderID]: EdgeKIOSK")
```
```

This rule will find all devices with the ZTDID and exclude all devices where the value of OrderID is EdgeKIOSK:

![](bbox=[0.133, 0.258, 0.864, 0.49])

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