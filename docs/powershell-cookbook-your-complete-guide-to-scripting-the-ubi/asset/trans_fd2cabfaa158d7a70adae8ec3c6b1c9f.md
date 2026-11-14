
## Invoke-WindowsApi 示例

```powershell
##############################################################################
##
## Invoke-WindowsApi
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

调用一个接受并返回简单数据类型的本机 Windows API。

.EXAMPLE

## 为 CreateHardLink 函数准备参数类型和参数
PS > $filename = "c:\temp\hardlinked.txt"
PS > $existingFilename = "c:\temp\link_target.txt"
PS > Set-Content $existingFilename "Hard Link target"
PS > $parameterTypes = [string], [string], [IntPtr]
PS > $parameters = [string] $filename, [string] $existingFilename,
    [IntPtr]::Zero

## 调用 Kernel32 DLL 中的 CreateHardLink 方法
PS > $result = Invoke-WindowsApi "kernel32" ([bool]) "CreateHardLink" `
    $parameterTypes $parameters
PS > Get-Content C:\temp\hardlinked.txt
Hard Link target

#>

param(
    ## 包含 Windows API 的 DLL 名称，例如 "kernel32"
    [string] $DllName,

    ## Windows API 期望的返回类型
    [Type] $ReturnType,

    ## Windows API 的名称
    [string] $MethodName,

    ## Windows API 期望的参数类型
    [Type[]] $ParameterTypes,

    ## 传递给 Windows API 的参数值
    [Object[]] $Parameters
)

Set-StrictMode -Version 3

## 开始构建动态程序集
$domain = [AppDomain]::CurrentDomain
$name = New-Object Reflection.AssemblyName 'PInvokeAssembly'
$assembly = $domain.DefineDynamicAssembly($name, 'Run')
$module = $assembly.DefineDynamicModule('PInvokeModule')
$type = $module.DefineType('PInvokeType', "Public,BeforeFieldInit")

## 遍历传递给我们的所有参数。在此过程中，
## 我们将用户的输入克隆到另一个数组中，该数组将用于
## P/Invoke 调用。
$inputParameters = @()
$refParameters = @()

for($counter = 1; $counter -le $parameterTypes.Length; $counter++)
{
    ## 如果某项是 PSReference，则表示用户
    ## 想要一个 [out] 参数。
    if($parameterTypes[$counter - 1] -eq [Ref])
    {
        ## 记录哪些参数用作 [Out] 参数
        $refParameters += $counter

        ## 在克隆的数组上，我们将 PSReference 类型替换为
        ## 表示 PSReference 值的 .NET 引用类型，
        ## 并将值替换为 PSReference 所持有的值。
        $parameterTypes[$counter - 1] =
            $parameters[$counter - 1].Value.GetType().MakeByRefType()
        $inputParameters += $parameters[$counter - 1].Value
    }
    else
    {
        ## 否则，只需将其实际参数添加到
        ## 输入数组中。
        $inputParameters += $parameters[$counter - 1]
    }
}

## 定义实际的 P/Invoke 方法，为最初为 [Ref]
## 的参数添加 [Out] 特性。
$method = $type.DefineMethod(
    $methodName, 'Public,HideBySig,Static,PinvokeImpl',
    $returnType, $parameterTypes)

foreach($refParameter in $refParameters)
{
    [void] $method.DefineParameter($refParameter, "Out", $null)
}

## 应用 P/Invoke 构造函数
$ctor = [Runtime.InteropServices.DllImportAttribute].GetConstructor([string])
$attr = New-Object Reflection.Emit.CustomAttributeBuilder $ctor, $dllName
$method.SetCustomAttribute($attr)

## 创建临时类型，并调用该方法。
$realType = $type.CreateType()

$realType.InvokeMember(
    $methodName, 'Public,Static,InvokeMethod', $null, $null,$inputParameters)

## 最后，遍历所有引用参数，并更新
## 用户传入的 PSReference 对象的值。
foreach($refParameter in $refParameters)
{
    $parameters[$refParameter - 1].Value = $inputParameters[$refParameter - 1]
}
```

有关运行脚本的详细信息，请参阅[食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 17.6 定义或扩展 .NET 类

## 问题

你想要定义一个新的 .NET 类或扩展现有类。

## 解决方案

在大多数情况下，你可以直接在 PowerShell 中定义 .NET 类：

```powershell
class GreatnessComparer : System.Collections.IComparer
{
    [int] Compare([Object] $X, [Object] $Y)
    {
        if($X -eq "Powershell") { return -1 }
        elseif($Y -eq "Powershell") { return 1 }
        else { return [String]::Compare($X, $Y) }
    }
}
```

```powershell
PS > $languages = "Perl","Ruby","Python","PowerShell","VBScript"
PS > [Array]::Sort($languages, [GreatnessComparer]::New())
PS > $languages

PowerShell
Perl
Python
Ruby
VBScript
```

对于其他情况，请使用 `Add-Type` 类的 `-TypeDefinition` 参数，如[示例 17-5](#ch17.xhtml#invoke-addtypetypedefinitionps1)所示。

### 示例 17-5. Invoke-AddTypeTypeDefinition.ps1

```powershell
#############################################################################
##
## Invoke-AddTypeTypeDefinition
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

演示 Add-Type cmdlet 的 -TypeDefinition 参数的用法。

#>

Set-StrictMode -Version 3

## 定义新的 C# 类
$newType = @'
using System;

namespace PowerShellCookbook
{
    public class AddTypeTypeDefinitionDemo
    {
        public string SayHello(string name)
        {
            string result = String.Format("Hello {0}", name);
            return result;
        }
    }
}

'@

## 将其添加到 PowerShell 会话
Add-Type -TypeDefinition $newType

## 显示我们可以像访问任何其他 .NET 类型一样访问它
$greeter = New-Object PowerShellCookbook.AddTypeTypeDefinitionDemo
$greeter.SayHello("World")
```

## 讨论

在大多数 PowerShell 场景中，如果你想要为数据添加结构或提供操作该数据的方法，可以使用 PowerShell 类来提供此功能。有关编写 PowerShell 类的详细信息，请参阅[示例 3-8](#ch03.xhtml#creating_a_custom_object)。有关使用 PowerShell 扩展 .NET 类的另一个示例，请参阅[食谱 3.7](#ch03.xhtml#program_create_dynamic_variables)。

对于其他场景，`Add-Type` cmdlet 是 PowerShell *胶水般* 特性的主要体现之一。它提供了几种与 .NET Framework 深度交互的独特方式。其主要操作模式之一源于 `-TypeDefinition` 参数，该参数允许你定义全新的 .NET 类。除了解决方案中给出的示例外，[食谱 3.7](#ch03.xhtml#program_create_dynamic_variables) 还演示了该技术的有效用法。

调用 `Add-Type` cmdlet 后，PowerShell 会将你提供的源代码编译成真正的 .NET 类。此操作等同于在传统开发环境（如 Visual Studio）中定义类，功能同样强大。

### 注意

在脚本执行过程中编译源代码的想法可能会让你担心性能影响。幸运的是，PowerShell 在编译时会保存这些对象。如果你在同一会话中使用相同的源代码第二次调用 `Add-Type` cmdlet，PowerShell 将重用第一次调用的结果。如果你想要更改已加载类型的行为，请退出会话并重新创建。

PowerShell 假定提供给 `-TypeDefinition` 参数的源代码默认语言为 C#。除了 C#，`Add-Type` cmdlet 还支持 C# 3.0 版本（LINQ、`var` 关键字等）、Visual Basic 和 JScript。它还支持实现 .NET 标准 CodeProvider 要求的语言（如 F#）。
