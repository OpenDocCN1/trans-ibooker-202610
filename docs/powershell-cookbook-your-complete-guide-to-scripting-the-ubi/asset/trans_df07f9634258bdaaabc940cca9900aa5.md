
`Add-Member` cmdlet 在帮助您向单个对象添加自定义成员方面非常有用。例如，假设您想根据当前目录中的文件创建一份报表，且该报表应包含每个文件的所有者。`Get-ChildItem` 生成的对象上没有标准的 `Owner` 属性，但您可以编写一个小脚本来添加它们，如 [示例 3-7](#ch03.xhtml#a_script_that_adds_custom_properties_to_) 所示。

### 示例 3-7. 向其文件对象输出添加自定义属性的脚本

```
##############################################################################
##
## Get-OwnerReport
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

获取当前目录中的文件列表，但将所有者添加到生成的对象中。

.EXAMPLE

PS > Get-OwnerReport | Format-Table Name,LastWriteTime,Owner
检索当前目录中的所有文件，并显示
名称、最后写入时间和所有者

#>

Set-StrictMode -Version 3

$files = Get-ChildItem
foreach($file in $files)
{
    $owner = (Get-Acl $file).Owner
    $file | Add-Member NoteProperty Owner $owner
    $file
}
```

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

要添加到对象的最常见信息类型是 `NoteProperty` 中的静态信息。如果您省略它，`Add-Member` 甚至将其用作默认值：

```
PS > $item = Get-Item C:\
PS > $item | Add-Member VolumeName "Operating System"
PS > $item.VolumeName
Operating System
```

除了 note 属性外，`Add-Member` cmdlet 还支持其他几种属性和方法类型，包括 `AliasProperty`、`ScriptProperty`、`CodeProperty`、`CodeMethod` 和 `ScriptMethod`。有关这些其他属性类型的更详细说明，请参阅 [“使用 .NET Framework”](#app01.xhtml#working_with_the_net_framework) 以及 `Add-Member` cmdlet 的帮助文档。

#### 注意

要创建全新的对象（而不是向现有对象添加信息），请参阅 [食谱 3.15](#ch03.xhtml#create_and_initialize_custom_objects)。

虽然 `Add-Member` cmdlet 允许您自定义特定对象，但它不允许您自定义该类型的所有对象。有关如何执行此操作的信息，请参阅 [食谱 3.16](#ch03.xhtml#add_custom_methods_and_properties_to_typ)。

### 计算属性

`Calculated` 属性是向输出对象添加信息的另一种有用方法。如果您的脚本或命令使用 `Format-Table` 或 `Select-Object` 命令生成输出，则可以通过提供生成其值的表达式来创建其他属性。例如：

```
Get-ChildItem |
  Select-Object Name,
      @{Name="Size (MB)"; Expression={ "{0,8:0.00}" -f ($_.Length / 1MB) } }
```

在此命令中，我们获取目录中的文件列表。我们使用 `Select-Object` 命令检索其名称和一个名为 `Size (MB)` 的计算属性。此计算属性以兆字节为单位返回文件大小，而不是默认值（字节）。

#### 注意

`Format-Table` cmdlet 支持类似的哈希表来添加计算属性，但使用 `Label`（而不是 `Name`）作为键来标识属性。为了消除由此产生的混淆，PowerShell 团队更新了这两个 cmdlet，使它们同时接受 `Name` 和 `Label`。

有关 `Add-Member` cmdlet 的更多信息，请键入 `Get-Help Add-Member`。

有关添加计算属性的更多信息，请键入 `Get-Help Select-Object` 或 `Get-Help Format-Table`。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[食谱 3.15，“创建并初始化自定义对象”](#ch03.xhtml#create_and_initialize_custom_objects)

[食谱 3.16，“向类型添加自定义方法和属性”](#ch03.xhtml#add_custom_methods_and_properties_to_typ)

[“使用 .NET Framework”](#app01.xhtml#working_with_the_net_framework)

# 3.15 创建并初始化自定义对象

## 问题

您希望从命令返回结构化结果，以便用户可以轻松地对它们进行排序、分组和筛选。

## 解决方案

使用 `[PSCustomObject]` 类型转换创建一个新的 `PSCustomObject`，并提供一个包含自定义信息的哈希表作为其值，如 [示例 3-8](#ch03.xhtml#creating_a_custom_object) 所示。

### 示例 3-8. 创建自定义对象

```
$output = [PSCustomObject] @{
    'User' = 'DOMAIN\User';
    'Quota' = 100MB;
    'ReportDate' = Get-Date;
}
```

如果您想创建一个带有关联功能的自定义对象，请在模块中编写一个 PowerShell 类，并创建该类的实例：

```
using module c:\modules\PlottingObject.psm1

$obj = [PlottingObject]::new()
$obj.Move(10,10)
$obj.Points = SineWave
while($true) { $obj.Rotate(10); $obj.Draw(); Sleep -m 20 }
```

## 讨论

当您的脚本向用户输出信息时，始终应优先考虑结构丰富的数据，而非手工格式化的报表。通过输出自定义对象，您赋予最终用户对脚本输出的控制权，就像 PowerShell 赋予您对其自身命令输出的控制权一样。

尽管自定义对象输出功能强大，但用户编写的脚本经常继续生成纯文本输出。部分原因要归咎于 PowerShell 以前对创建和初始化自定义对象的繁琐支持，如 [示例 3-9](#ch03.xhtml#version_one_syntax_for_custom_objects) 所示。

### 示例 3-9. 在 PowerShell 版本 1 中创建自定义对象

```
$output = New-Object PsObject
Add-Member -InputObject $output NoteProperty User 'DOMAIN\user'
Add-Member -InputObject $output NoteProperty Quota 100MB
Add-Member -InputObject $output NoteProperty ReportDate (Get-Date)

$output
```

在 PowerShell 版本 1 中，创建自定义对象需要创建一个新对象（类型为 `PsObject`），然后多次调用 `Add-Member` cmdlet 以添加所需的属性。PowerShell 版本 2 通过向 `New-Object` cmdlet 添加 `-Property` 参数，使这变得容易得多，该参数也适用于 `PSObject` 类型。PowerShell 版本 3 通过直接支持 `[PSCustomObject]` 类型转换，使这变得尽可能简单。

虽然创建 `PSCustomObject` 使创建以数据为中心的对象（通常称为 `property bags`）变得容易，但它不允许您向这些对象添加功能。当您也需要功能时，下一步是创建一个 PowerShell 类（请参阅 [示例 3-10](#ch03.xhtml#module_exporting_class)）。与许多其他语言一样，PowerShell 类支持构造函数、公共属性和公共方法，以及内部辅助变量和方法。

### 示例 3-10. 创建导出自定义类的模块
