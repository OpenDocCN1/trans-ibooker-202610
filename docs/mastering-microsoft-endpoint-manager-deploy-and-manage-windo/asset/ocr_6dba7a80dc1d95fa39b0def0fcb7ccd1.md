+   - ModelName: This is the value specified by the OEM in the SMBIOS firmware of the device for the particular model, for example, *Surface Pro 7*.

To register Windows Autopilot devices manually, the process is as follows.

You can have up to 500 rows in the CSV file. The header and line format are shown next, which include the device serial number, the Windows product ID, the hardware hash, the group tag, and the assigned user:

```
<serialNumber>,<ProductID>,<hardwareHash>,
<optionalGroupTag>,<optionalAssignedUser>
```

> **Important Note**
> Import Windows Autopilot devices from a CSV file. When assigning users in the CSV file, make sure that you are assigning the correct **User Principal Name (UPN)** as there is no validation of UPNs during the import process. This means that if an incorrect UPN is in the CSV file, a user will not be assigned to the Windows Autopilot device object.

Assigning a user to a specific Autopilot device does not work if you are using **Active Directory Federation Services (ADFS)**.

# Windows Autopilot Deployment Program

![](bbox=[0.13, 0.56, 0.86, 0.71])

Figure 7.6 – Windows Autopilot Deployment Program

Go to the Microsoft Endpoint Manager admin center, https://endpoint.microsoft.com/, and choose Devices | Windows | Windows enrollment | Devices.