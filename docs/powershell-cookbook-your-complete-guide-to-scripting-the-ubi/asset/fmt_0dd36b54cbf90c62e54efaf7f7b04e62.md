
For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

## 18.16 Add and Remove Certificates

### Problem

You want to add and remove certificates in the certificate store.

### Solution

To remove a certificate, use the `Remove-Item` cmdlet. For example, to remove temporary certificates that you create when debugging SSL websites with the Fiddler HTTP debugging proxy:

```
PS Cert:\CurrentUser\My > dir |
    Where Subject -like "*OU=Created by http://www.fiddler2.com" | Remove-Item
```

To add a certificate, use the certificate store APIs from the .NET Framework, as shown in [Example 18-7](#ch18.xhtml#adding_certificates).

### Example 18-7. Adding certificates

```
## Adding a certificate from disk
$cert = Get-PfxCertificate <path_to_certificate>
$store = New-Object System.Security.Cryptography.X509Certificates.X509Store `
    "TrustedPublisher","CurrentUser"
$store.Open("ReadWrite")
$store.Add($cert)
$store.Close()
```

### Discussion

The certificate drive provides a useful way to navigate and view certificates for the current user or local machine. For example, if your execution policy requires the use of digital signatures, the following command tells you which publishers are trusted to run scripts on your system:

```
Get-ChildItem cert:\CurrentUser\TrustedPublisher
```

If you want to remove a trusted publisher from this store, simply use the `Remove-Item` cmdlet to do so.

While it’s easy to remove a certificate, adding a certificate is not as easy. For example, the `Get-PfxCertificate` cmdlet lets you review a certificate from a file that contains it, but it doesn’t let you install it into the certificate store permanently. The .NET APIs provide the way to import the certificate for good.

For more information about retrieving certificates from the certificate provider, please see [Recipe 18.14](#ch18.xhtml#access_user_and_machine_certificates). For more information about working with classes from the .NET Framework, please see [Recipe 3.8](#ch03.xhtml#work_with_net_objects).

### See Also

[Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)

[Recipe 18.14, “Access User and Machine Certificates”](#ch18.xhtml#access_user_and_machine_certificates)

## 18.17 Manage Security Descriptors in SDDL Form

### Problem

You want to work with a security identifier in Security Descriptor Definition Language (SDDL) form.

### Solution

Use the `ConvertFrom-SddlString` cmdlet to see the human-readable version of a SDDL string:

```
PS > ConvertFrom-SddlString (Get-Acl C:\).Sddl

Owner            : NT SERVICE\TrustedInstaller
Group            : NT SERVICE\TrustedInstaller
DiscretionaryAcl : {NT AUTHORITY\Authenticated Users: AccessAllowed
                   (CreateDirectories), NT
                   AUTHORITY\SYSTEM: AccessAllowed (ChangePermissions,
                   CreateDirectories, Delete, DeleteSubdirectoriesAndFiles,
                   ExecuteKey, FullControl, FullControl, FullControl, FullControl,
                   GenericAll, GenericExecute, GenericRead, GenericWrite,
                   ListDirectory, Modify, Read, ReadAndExecute, ReadAttributes,
                   ReadExtendedAttributes, ReadPermissions, Synchronize,
                   TakeOwnership, Traverse, Write, WriteAttributes, WriteData,
                   WriteExtendedAttributes, WriteKey),
                   BUILTIN\Administrators: AccessAllowed (ChangePermissions,
                   CreateDirectories, Delete, DeleteSubdirectoriesAndFiles,
                   ExecuteKey, FullControl, FullControl, FullControl, FullControl,
                   GenericAll, GenericExecute, GenericRead, GenericWrite,
                   ListDirectory, Modify, Read, ReadAndExecute, ReadAttributes,
                   ReadExtendedAttributes, ReadPermissions, Synchronize,
                   TakeOwnership, Traverse, Write, WriteAttributes, WriteData,
                   WriteExtendedAttributes, WriteKey),
                   BUILTIN\Users: AccessAllowed (GenericWrite, ListDirectory, Read,
                   ReadAndExecute, ReadAttributes, ReadExtendedAttributes,
                   ReadPermissions, Synchronize, Traverse)}
SystemAcl        : {}
RawDescriptor    : System.Security.AccessControl.CommonSecurityDescriptor
```

Use the `System.Security.AccessControl.CommonSecurityDescriptor` class from the .NET Framework, as shown by [Example 18-8](#ch18.xhtml#automating-security-configuration-2).

### Example 18-8. Automating security configuration of the PowerShell Remoting Users group

```
## Get the SID for the "PowerShell Remoting Users" group
$account = New-Object Security.Principal.NTAccount "PowerShell Remoting Users"
$sid = $account.Translate([Security.Principal.SecurityIdentifier]).Value

## Get the security descriptor for the existing configuration
$config = Get-PSSessionConfiguration Microsoft.PowerShell
$existingSddl = $config.SecurityDescriptorSddl

## Create a CommonSecurityDescriptor object out of the existing SDDL
## so that we don't need to manage the string by hand
$arguments = $false,$false,$existingSddl
$mapper = New-Object Security.AccessControl.CommonSecurityDescriptor $arguments

## Create a new access rule that adds the "PowerShell Remoting Users" group
$mapper.DiscretionaryAcl.AddAccess("Allow",$sid,268435456,"None","None")

## Get the new SDDL for that configuration
$newSddl = $mapper.GetSddlForm("All")

## Update the endpoint configuration
Set-PSSessionConfiguration Microsoft.PowerShell -SecurityDescriptorSddl $newSddl
```

### Discussion

Security descriptors are often shown (or requested) in SDDL form. The SDDL form of a security descriptor is cryptic, highly specific, and plain text. All of these aspects make this format difficult to work with reliably, so you can use the `System.Security.AccessControl.CommonSecurityDescriptor` class from the .NET Framework to do most of the gritty work for you.

For more information about the SDDL format, see [the Microsoft documentation](https://oreil.ly/WlRkX). For an example of this in action, see [Recipe 29.12](#ch29.xhtml#configure_user_permissions_remoting).

### See Also

[Recipe 29.12, “Configure User Permissions for Remoting”](#ch29.xhtml#configure_user_permissions_remoting)

## 18.18 Create a Task-Specific Remoting Endpoint

### Problem

You want to create a PowerShell Remoting endpoint that lets authorized users accomplish limited administrative tasks on a machine without requiring that they be a full administrator of that machine.

### Solution

Use the `New-PSSessionConfigurationFile` command to create a session configuration, and then use the `Register-PSSessionConfiguration` cmdlet to create an endpoint based on that configuration.
