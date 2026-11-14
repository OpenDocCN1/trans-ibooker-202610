
## Discussion

除了提供功能完整且丰富的远程终结点这一主要特性外，PowerShell 还允许你将会话配置为另一种极端模式。这是通过一项称为 `Just Enough Administration` (JEA) 的功能实现的。JEA 允许你控制向用户公开哪些命令，创建代理函数以用更安全的版本封装命令，并完全移除对 PowerShell 语言的访问权限。

在 PowerShell 的早期版本中，实现特定任务的终结点是一项繁重的工作，需要一个开发团队才能完成：创建自定义程序集、构建 `initial session state`、将所有这些托管在 Web 服务器内，等等。然而，这非常强大，并构成了 Exchange Online 和 SharePoint Online 等极其成功的远程管理系统的基础。

但通过 JEA 的功能，创建这些特定任务的终结点以限制环境中高度特权管理员的蔓延，变得非常容易。事实上，JEA 已被用于限制对核反应堆的访问、限制梅赛德斯一级方程式车队 IT 部门的管理权限蔓延等。在某个案例中，一家公司成功将其域管理员总数从近 300 人减少到仅 3 人！

JEA 的基础在于 PowerShell 理解两种类型的命令概念：`public` 和 `private`。主要区别在于用户只能调用公共命令，而公共命令可以在内部调用公共和私有命令。这使你可以编写一个公共函数，例如，在内部调用许多私有 PowerShell cmdlet 来完成其任务。

开始使用 JEA 的第一步是创建会话配置文件。解决方案演示了一个 `Inventory` 终结点的框架，该终结点仅公开 `Get-Inventory` 命令。该 cmdlet 最关键的三个参数是 `-SessionType` `RestrictedRemoteServer`（设置安全默认值，使其适合 JEA 终结点）、`-RunAsVirtualAccount`（允许非管理员用户完成需要管理凭据的任务）和 `-RoleDefinitions`（告诉 PowerShell 你的终结点应允许的实际命令）。

会话配置文件允许你调整 JEA 终结点的许多其他方面：用于安全记录的目录、为文件传输挂载临时用户驱动器的能力，等等。

### Warning

创建 JEA 配置时，请勿更改 `SessionType` 或 `LanguageMode` 参数。这会引入安全漏洞（例如允许用户定义自己的函数），从而让他们攻破托管你的 JEA 终结点的服务器。

当用户连接到你的 JEA 终结点时，PowerShell 会呈现你为该用户允许的任何命令。最简单的方法是通过特定于每个可能角色的脚本模块来实现——例如我们在示例中创建的 `InventoryReader` 模块。你可以充实此模块以实现任何你认为合适的功能，然后使用 `Export-ModuleMember` cmdlet 选择 PowerShell 应向连接用户提供的命令。一旦你为每个角色实现了一个模块，`-RoleDefinitions` 参数就允许你告诉 PowerShell 哪些安全组应被允许访问哪些角色。虽然解决方案给出了一个安全组被授予访问一个角色的示例，但 PowerShell 也支持更丰富的行为。

在为各种角色创建模块时，请注意某些编码错误——主要是围绕代码注入的错误——很容易成为安全漏洞。例如，如果你有一个委托对 `Set-AdUser` cmdlet 访问权限的系统，并且你不安全地调用该命令，恶意输入很容易攻破你的域：

```powershell
function Set-PersonalDisplayName($DisplayName)
{
    $c = "Set-AdUser -UserPrincipalName " +
        "$($PSSenderInfo.UserInfo.Identity.Name) -DisplayName $DisplayName"
    Invoke-Expression $c
}
```

因为 `Invoke-Expression` 会运行你给它的所有代码（包括分号和其他代码），以下恶意输入将允许用户将自己添加到域管理员：

```powershell
Set-PersonalDisplayName 'Attacker; Add-GroupMember "Domain Admins" DOMAIN\Attacker'
```

该函数的安全实现如下：

```powershell
function Set-PersonalDisplayName($DisplayName)
{
    Set-AdUser -UserPrincipalName $PSSenderInfo.UserInfo.Identity.Name `
        -DisplayName $DisplayName
}
```

有关如何审核和保护自己免受代码注入风险的信息，请参阅 [配方 18.20](#ch18.xhtml#detect_prevent_code_injection)。

有关 PowerShell 远程处理的更多信息，请参阅 [第 29 章](#ch29.xhtml#chapter_remoting)。

## See Also

[配方 18.20，“检测和防止代码注入漏洞”](#ch18.xhtml#detect_prevent_code_injection)

[第 29 章](#ch29.xhtml#chapter_remoting)

# 18.19 限制 PowerShell 的交互式使用

## Problem

你有一台跳板机、自助终端或其他安全主机，并希望允许通过 PowerShell 进行有限度的管理。

## Solution

设置 `ConsoleSessionConfiguration` 注册表策略，强制所有本地 PowerShell 实例透明地连接到指定的 Just Enough Administration (JEA) 终结点。以下是一个强制所有交互式 Shell 连接到 `Jumpbox` 终结点的示例：

