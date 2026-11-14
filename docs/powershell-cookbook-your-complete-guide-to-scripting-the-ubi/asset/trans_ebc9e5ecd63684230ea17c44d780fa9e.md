

# 18.20 避免 Invoke-Expression 注入

## 讨论

解决方案中给出了一个非常简单的示例，其中用户输入通过变量扩展被不安全地混入到 `Invoke-Expression` 命令中：

```
function Set-PersonalDisplayName($DisplayName)
{
    $c = "Set-AdUser -User DOMAIN\user -DisplayName $DisplayName"
    Invoke-Expression $c
}
```

`Invoke-Expression` 是 PowerShell 的 cmdlet，它接受你给它的任何输入，将其视为 PowerShell 代码并运行。管理员在将其与用户输入结合使用时，脚本能正常工作往往只是运气好而已。让我们看一个简单的例子：

```
Set-PersonalDisplayName -DisplayName "James O'Neil"
```

此脚本运行：

```
Invoke-Expression "Set-AdUser -User DOMAIN\user -DisplayName James O'Neil"
```

这相当于在命令行运行以下命令：

```
Set-AdUser -User DOMAIN\user -DisplayName James O'Neil
```

然后你会开始接到服务台的电话，用户倒霉地姓氏中只包含一个左引号而没有相应的右引号：

```
Invoke-Expression:
Line |
   4 |      Invoke-Expression $c
     |      ~~~~~~~~~~~~~~~~~~~~
     | 字符串缺少终止符: '。
```

如果攻击者在特殊 PowerShell 字符的放置上更具选择性，他们可能会尝试类似这样的操作：

```
PS > Set-PersonalDisplayName -DisplayName "James'' Revenge; calc"
```

有关 `Invoke-Expression` 的风险以及更易用、更安全的替代方法的详细信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

PowerShell Gallery 中的 Injection Hunter 模块可让你检测此类问题，并提供关于如何编写更安全代码的建议。你甚至可以将其集成到 Visual Studio Code 中，以便在编写脚本时运行它，如 [图 18-4](#ch18.xhtml#running_script_analyzer) 所示。

![](Images/wps4_1804.png)

要执行此操作，请通过输入 `Ctrl+Comma` 打开 PowerShell 脚本分析器设置，然后输入 `analyzer`。在“Script Analyzer Settings Path”下，输入这些设置的路径——例如与你的配置文件放在一起：`d:\lee\PowerShell\PSScriptAnalyzer​Settings.psd1`。在其中放置以下内容：

```
@{
 IncludeDefaultRules = $true
 CustomRulePath =
     "D:\Lee\WindowsPowerShell\Modules\InjectionHunter\1.0.0\InjectionHunter.psd1"
}
```

对于 `CustomRulePath` 设置，你可以通过输入以下命令获取 Injection Hunter 的路径：

```
Get-Module InjectionHunter -List | ForEach-Object Path
```

Injection Hunter 可检测来自 Invoke Expression、危险 API 用法、命令注入、方法和属性篡改以及不安全输入转义的安全问题。

要了解如何自己编写分析规则，请参阅 [食谱 10.10](#ch10.xhtml#parse_and_interpret_powershell_scripts) 以开始入门。掌握了这一点后，Injection Hunter 是作为脚本模块编写的。你可以阅读 `InjectionHunter.psm1` 的内容，了解它是如何实现现有规则的。

## 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[食谱 10.10，“解析和解释 PowerShell 脚本”](#ch10.xhtml#parse_interpret_powershell_scripts)

[食谱 18.18，“创建特定任务的远程终结点”](#ch18.xhtml#create_task_specific_endpoint)

# 18.21 获取文件的加密哈希值

## 问题

你想要验证文件的加密（MD5、SHA1、SHA2）哈希值。

## 解决方案

使用 `Get-FileHash` cmdlet：

```
$url = "https://github.com/PowerShell/PowerShell/releases/download/" +
    "v7.1.3/powershell-7.1.3-linux-arm64.tar.gz"
Invoke-WebRequest $url -OutFile powershell-7.1.3-linux-arm64.tar.gz
```

```
PS > Get-FileHash .\powershell-7.1.3-linux-arm64.tar.gz | Format-List

Algorithm : SHA256
Hash      : B4ECB166EBBD7232CDF2ED3CC84D90FF1A01E62F5307EB58868697CA6CB3B4A2
Path      : C:\temp\powershell-7.1.3-linux-arm64.tar.gz
```

要获取字符串或其他原始二进制内容的加密哈希值，请使用 `-InputStream` 参数：

```
$bytes = [Text.Encoding]::Unicode.GetBytes("My future prediction")
Get-FileHash -InputStream ([IO.MemoryStream] $bytes)
```

## 讨论

文件哈希提供了一种检查文件损坏或修改的有用方法。数字哈希就像文件的指纹，能检测到即使是微小的修改。如果文件内容发生变化，其哈希值也会随之改变。许多在线下载服务在该文件的下载页面上提供文件的哈希值，以便你确定传输是否以某种方式损坏了文件（请参阅 [图 18-5](#ch18.xhtml#file_hashes_as_a_verification_mechanism)）。

![](Images/wps4_1805.png)

生成文件哈希有三种常见方式：MD5、SHA1 和 SHA256。最常用的是 MD5，其次是 SHA1。虽然流行，但这些哈希类型只能受信任用于检测意外的文件修改。如果有人想在不改变文件哈希的情况下篡改文件，它们可能会被欺骗。SHA256 算法可用于防范甚至是有意的文件篡改。

`Get-FileHash` cmdlet 允许你确定文件的哈希值（如果通过管道提供多个文件，则可确定多个文件的哈希值）。

要高效地记录和比对大量文件集的文件哈希值，请参阅 [食谱 18.22](#ch18.xhtml#capture_verify_file_integrity)。

## 另请参阅

[食谱 18.22](#ch18.xhtml#capture_verify_file_integrity)

# 18.22 捕获并验证文件集的完整性

## 问题

你想要检测目录或已发布文件目录中的任何文件是否发生更改。

## 解决方案

使用 `New-FileCatalog` 和 `Test-FileCatalog` cmdlet：

```
## 为我们的目录创建一些文件
"Hello World 1" > unchanged.txt
"Hello World 2" > tamper_target.txt
"Hello World 3" > delete_target.txt

## 创建目录
New-FileCatalog catalog.cat

## 更改一个文件，删除一个文件，并创建一个新文件
"Hello World Tampered" > tamper_target.txt
Remove-item delete_target.txt
"Hello World 5" > newfile.txt
```

```
## 测试文件目录
PS > Test-FileCatalog catalog.cat -Detailed

Status        : ValidationFailed
HashAlgorithm : SHA256
CatalogItems  : {[delete_target.txt,
                FD36E8BD105A56796600C6CE8697685FE520F3AA154497AFA6B849C8A3E73B14],
                [tamper_target.txt,
                678DF656B70CC98AD0D00AC5B7C8FE3A96B9C1B7040EFCC7610C3FE4987A45DA],
                [unchanged.txt,
                216A9860C6B4E0149B27C127DB443A11199375DADC388BD3401A00D191E037B5]}
PathItems     : {[newfile.txt,
                F720B1FD7899F6AE3FED8C6640A36D61AB95BD794CA530CACA70D5D1603CA9E7],
                [tamper_target.txt,
                1713559C3BDEBAC97A5B073B72F59F1AC2A9082DFDC64048A64C86B7A835E434],
                [unchanged.txt,
                216A9860C6B4E0149B27C127DB443A11199375DADC388BD3401A00D191E037B5]}
Signature     : System.Management.Automation.Signature
```

## 讨论

文件目录是 Windows 中的一种机制，允许你验证文件列表的完整性。当你使用 `New-FileCatalog` cmdlet 创建文件目录时，PowerShell 会从当前目录树中捕获文件名和文件哈希值，并将它们包含在文件目录中。

当你稍后想要验证文件目录时，`Test-FileCatalog` cmdlet 允许你比对所有文件及其文件哈希值，以确定是否有任何内容发生更改，以及是否有文件被添加或删除。

你可以使用 `Get-AuthenticodeSignature` 和 `Set-AuthenticodeSignature` cmdlet 来保护文件目录本身的完整性。

虽然 `Test-FileCatalog` cmdlet 会告诉你是否有任何内容发生了更改，但还需要一个额外的步骤来查看确切发生了什么更改。你可以使用 `Compare-Object` cmdlet 来查看目录中的内容与给定路径中的内容之间的差异：

```
$catalog = Test-FileCatalog catalog.cat -Detailed
$catalogItems = @($catalog.CatalogItems.GetEnumerator())
$pathItems = @($catalog.PathItems.GetEnumerator())
```

