
有关与 Windows API 交互的详细信息，请参阅 [食谱 17.4](#ch17.xhtml#access_windows_api_functions)。有关运行脚本的详细信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[食谱 17.4，“访问 Windows API 函数”](#ch17.xhtml#access_windows_api_functions)

# 20.20 获取文件或目录的 ACL

## 问题

你想要检索文件或目录的 ACL。

## 解决方案

要检索文件的 ACL，请使用 `Get-Acl` cmdlet：

```
PS > Get-Acl example.txt

      Directory: C:\temp

Path                       Owner                     Access
----                       -----                     ------
example.txt                LEE-DESK\Lee              BUILTIN\Administrator...
```

## 讨论

`Get-Acl` cmdlet 检索项目的安全描述符。然而，此 cmdlet 不仅适用于文件系统。任何支持安全描述符概念的提供程序（例如注册表提供程序）也支持 `Get-Acl` cmdlet。

`Get-Acl` cmdlet 返回一个表示项目安全描述符的对象，该对象特定于包含该项目的提供程序。在文件系统中，这将返回一个 .NET `System.Security.AccessControl.FileSecurity` 对象，你可以通过探索它来获取更多信息。例如，[示例 20-4](#ch20.xhtml#get-aclmisconfigurationps1) 通过确保每个文件都包含一个“管理员，完全控制” ACL，来搜索目录中可能的 ACL 错误配置。

### 示例 20-4. Get-AclMisconfiguration.ps1

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

有关 `Get-Acl` 命令的详细信息，请输入 `Get-Help Get-Acl`。有关使用 .NET Framework 类的详细信息，请参阅 [食谱 3.8](#ch03.xhtml#work_with_net_objects)。有关运行脚本的详细信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[食谱 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

# 20.21 设置文件或目录的 ACL

## 问题

你想要更改文件或目录的 ACL。

## 解决方案

要更改文件的 ACL，请使用 `Set-Acl` cmdlet。此示例阻止 `Guest` 帐户访问文件：

```
$acl = Get-Acl example.txt
$arguments = "LEE-DESK\Guest","FullControl","Deny"
$accessRule =
    New-Object System.Security.AccessControl.FileSystemAccessRule $arguments
$acl.SetAccessRule($accessRule)
$acl | Set-Acl example.txt
```

## 讨论

`Set-Acl` cmdlet 设置项目的安全描述符。然而，此 cmdlet 不仅适用于文件系统。任何支持安全描述符概念的提供程序（例如注册表提供程序）也支持 `Set-Acl` cmdlet。

`Set-Acl` cmdlet 要求你提供一个要应用于项目的 ACL。虽然可以从头构建 ACL，但通常最简单的方法是预先从项目中检索它（如解决方案中所示）。要检索 ACL，请使用 `Get-Acl` cmdlet。一旦你修改了 ACL 上的访问控制规则，只需将它们通过管道传递给 `Set-Acl` cmdlet 即可使其永久生效。

在解决方案中，我们提供给 `FileSystemAccessRule` 构造函数的 `$arguments` 列表在 `LEE-DESK` 计算机的 `Guest` 帐户上为 `FullControl` 权限显式设置了一条 `Deny` 规则。有关使用 .NET Framework 类（如 `FileSystemAccessRule` 类）的详细信息，请参阅 [食谱 3.8](#ch03.xhtml#work_with_net_objects)。

虽然 `Set-Acl` 命令功能强大，但你可能已经熟悉提供类似功能的命令行工具（如 `cacls.exe`）。虽然这些工具通常不适用于注册表（或其他支持 PowerShell 安全描述符的提供程序），但你当然可以继续在 PowerShell 中使用这些工具。

有关 `Set-Acl` cmdlet 的详细信息，请输入 `Get-Help Set-Acl`。有关 `Get-Acl` cmdlet 的详细信息，请参阅 [食谱 20.20](#ch20.xhtml#get_the_acl_of_a_file_or_directory)。

## 另请参阅

[食谱 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

[食谱 20.20，“获取文件或目录的 ACL”](#ch20.xhtml#get_the_acl_of_a_file_or_directory)

# 20.22 程序：向文件添加扩展文件属性

当你单击文件的“属性”对话框时，资源管理器 Shell 会提供关于该文件的有用信息。它包括创作信息、图像信息、音乐信息等（请参阅 [图 20-1](#ch20.xhtml#extended_file_properties_in_windows_expl)）。

PowerShell 默认不公开此信息，但可以从 `Shell.Application` COM 对象获取这些属性。[示例 20-5](#ch20.xhtml#add-extendedfilepropertiesps1) 正是执行此操作——并将此扩展信息作为属性添加到 `Get-ChildItem` cmdlet 返回的文件中。

![](Images/wps4_2001.png)

### 示例 20-5. Add-ExtendedFileProperties.ps1
