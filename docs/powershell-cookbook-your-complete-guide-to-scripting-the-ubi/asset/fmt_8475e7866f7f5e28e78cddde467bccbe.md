
For more information about interacting with the Windows API, see [Recipe 17.4](#ch17.xhtml#access_windows_api_functions). For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 17.4, “Access Windows API Functions”](#ch17.xhtml#access_windows_api_functions)

# 20.20 Get the ACL of a File or Directory

## Problem

You want to retrieve the ACL of a file or directory.

## Solution

To retrieve the ACL of a file, use the `Get-Acl` cmdlet:

```
PS > Get-Acl example.txt

      Directory: C:\temp

Path                       Owner                     Access
----                       -----                     ------
example.txt                LEE-DESK\Lee              BUILTIN\Administrator...
```

## Discussion

The `Get-Acl` cmdlet retrieves the security descriptor of an item. This cmdlet doesn’t work against only the filesystem, however. Any provider (for example, the registry provider) that supports the concept of security descriptors also supports the `Get-Acl` cmdlet.

The `Get-Acl` cmdlet returns an object that represents the security descriptor of the item and is specific to the provider that contains the item. In the filesystem, this returns a .NET `System.Security.AccessControl.FileSecurity` object that you can explore for further information. For example, [Example 20-4](#ch20.xhtml#get-aclmisconfigurationps1) searches a directory for possible ACL misconfigurations by ensuring that each file contains an Administrator, Full Control ACL.

### Example 20-4. Get-AclMisconfiguration.ps1

```
##############################################################################
##
## Get-AclMisconfiguration
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Demonstration of functionality exposed by the Get-Acl cmdlet. This script
goes through all access rules in all files in the current directory, and
ensures that the Administrator group has full control of that file.

#>

Set-StrictMode -Version 3

## Get all files in the current directory
foreach($file in Get-ChildItem)
{
    ## Retrieve the ACL from the current file
    $acl = Get-Acl $file
    if(-not $acl)
    {
        continue
    }

    $foundAdministratorAcl = $false

    ## Go through each access rule in that ACL
    foreach($accessRule in $acl.Access)
    {
        ## If we find the Administrator, Full Control access rule,
        ## then set the $foundAdministratorAcl variable
        if(($accessRule.IdentityReference -like "*Administrator*") -and
            ($accessRule.FileSystemRights -eq "FullControl"))
        {
            $foundAdministratorAcl = $true
        }
    }

    ## If we didn't find the administrator ACL, output a message
    if(-not $foundAdministratorAcl)
    {
        "Found possible ACL Misconfiguration: $file"
    }
}
```

For more information about the `Get-Acl` command, type `Get-Help Get-Acl`. For more information about working with classes from the .NET Framework, see [Recipe 3.8](#ch03.xhtml#work_with_net_objects). For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)

# 20.21 Set the ACL of a File or Directory

## Problem

You want to change the ACL of a file or directory.

## Solution

To change the ACL of a file, use the `Set-Acl` cmdlet. This example prevents the `Guest` account from accessing a file:

```
$acl = Get-Acl example.txt
$arguments = "LEE-DESK\Guest","FullControl","Deny"
$accessRule =
    New-Object System.Security.AccessControl.FileSystemAccessRule $arguments
$acl.SetAccessRule($accessRule)
$acl | Set-Acl example.txt
```

## Discussion

The `Set-Acl` cmdlet sets the security descriptor of an item. This cmdlet doesn’t work against only the filesystem, however. Any provider (for example, the registry provider) that supports the concept of security descriptors also supports the `Set-Acl` cmdlet.

The `Set-Acl` cmdlet requires that you provide it with an ACL to apply to the item. While it’s possible to construct the ACL from scratch, it’s usually easiest to retrieve it from the item beforehand (as demonstrated in the Solution). To retrieve the ACL, use the `Get-Acl` cmdlet. Once you’ve modified the access control rules on the ACL, simply pipe them to the `Set-Acl` cmdlet to make them permanent.

In the Solution, the `$arguments` list that we provide to the `FileSystemAccessRule` constructor explicitly sets a `Deny` rule on the `Guest` account of the `LEE-DESK` computer for `FullControl` permission. For more information about working with classes from the .NET Framework (such as the `FileSystemAccessRule` class), see [Recipe 3.8](#ch03.xhtml#work_with_net_objects).

Although the `Set-Acl` command is powerful, you may already be familiar with command-line tools that offer similar functionality (such as `cacls.exe`). Although these tools generally don’t work on the registry (or other providers that support PowerShell security descriptors), you can, of course, continue to use these tools from PowerShell.

For more information about the `Set-Acl` cmdlet, type `Get-Help Set-Acl`. For more information about the `Get-Acl` cmdlet, see [Recipe 20.20](#ch20.xhtml#get_the_acl_of_a_file_or_directory).

## See Also

[Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)

[Recipe 20.20, “Get the ACL of a File or Directory”](#ch20.xhtml#get_the_acl_of_a_file_or_directory)

# 20.22 Program: Add Extended File Properties to Files

The Explorer shell provides useful information about a file when you click on its Properties dialog. It includes the authoring information, image information, music information, and more (see [Figure 20-1](#ch20.xhtml#extended_file_properties_in_windows_expl)).

PowerShell doesn’t expose this information by default, but it is possible to obtain these properties from the `Shell.Application` COM object. [Example 20-5](#ch20.xhtml#add-extendedfilepropertiesps1) does just that—and adds this extended information as properties to the files returned by the `Get-ChildItem` cmdlet.

![](Images/wps4_2001.png)

### Example 20-5. Add-ExtendedFileProperties.ps1
