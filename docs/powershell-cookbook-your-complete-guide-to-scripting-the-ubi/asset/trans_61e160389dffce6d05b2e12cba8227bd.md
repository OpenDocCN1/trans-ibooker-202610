
同样，如果该项目已作为独立脚本发布，请使用 `Find-Script` 和 `Install-Script` 命令。如果您尚未在计算机上执行此操作，请务必将 `My Documents\PowerShell\Scripts` 添加到系统路径中。有关修改系统路径的详细信息，请参阅 [Recipe 16.2](#ch16.xhtml#view_and_modify_system_path)。

```
PS > Find-Script Get-WordCluster | Install-Script -Scope CurrentUser
PS > Get-WordCluster -Count 3 "Hello","World","Jello",
    "Mellow","Jealous","Wordy","Sword"

代表项
-------------- -----
Wordd          {World, Wordy, Sword}
Jealou         {Jello, Jealous}
Hellow         {Hello, Mellow}
```

## 讨论

PowerShell 库是发布和共享 PowerShell 脚本及模块的全球中心。它包含数千个模块：微软和许多其他公司的官方企业版本、流行的社区项目（如用于 SQL 管理的 `DbaTools` 模块），以及有趣的异想天开的项目（如用于将图像显示为 ANSI 图形的 `OutConsolePicture`）。

PowerShell 库的 [Web 界面](https://www.powershellgallery.com) 允许您搜索、浏览和探索，但当然，这不是您通过 PowerShell 使用它的方式。

在 PowerShell 中，`Find-Module` 和 `Install-Module` 命令允许您与 PowerShell 库交互并从中安装模块。您可以按名称、标记甚至 Just Enough Administration 角色功能查找模块。

当您首次尝试从 PowerShell 库安装模块时，PowerShell 将显示警告：

```
PS > Install-Module someModule -Scope CurrentUser

不受信任的存储库
您正在从不受信任的存储库安装模块。如果您信任此
存储库，请通过运行 Set-PSRepository
cmdlet 更改其 InstallationPolicy 值。您确定要从 'PSGallery' 安装模块吗？
[Y] 是  [A] 全是  [N] 否  [L] 全否  [S] 挂起  [?] 帮助
(默认为 "N"):
```

与其他所有代码共享存储库一样，对于谁可以发布到 PowerShell 库以及可以发布什么内容，没有任何限制。如果某个模块通过滥用举报机制被举报，并被认定为恶意软件或违反库的服务条款，当然会被移除。但除此之外——您不应认为该库经过了审查、批准或以其他方式隐含地值得信任。为了确认这一点并消除未来模块安装时的警告，您可以在计算机上将 PowerShell 库声明为受信任：

```
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
```

除了公共 PowerShell 库之外，PowerShell 还可以与专用库（包括文件共享！）进行通信。PowerShell 使用 NuGet 协议。有关创建专用 PowerShell 库的详细信息，请参阅 [PowerShell Gallery documentation](https://oreil.ly/8sPaO)。

## 另请参阅

[Recipe 16.2, “修改用户或系统路径”](#ch16.xhtml#view_and_modify_system_path)

[PowerShell Gallery documentation](https://oreil.ly/8sPaO)

# 1.30 使用自定义 Shell 中的命令

## 问题

您想要在常规 PowerShell 会话中使用基于 PowerShell 的产品所提供的命令，该产品会启动一个自定义版本的 PowerShell 控制台。

## 解决方案

启动自定义版本的 PowerShell 控制台，然后使用 `Get-Module` 和 `Get-PsSnapin` 命令查看它加载了哪些额外的模块和/或管理单元。

## 讨论

正如 [Recipe 1.28](#ch01.xhtml#extend_your_shell_with_additional_commands) 所述，PowerShell 模块和管理单元是第三方分发和添加额外 PowerShell 命令的两种方式。提供自定义版 PowerShell 控制台的产品通过使用以下三个参数之一启动 PowerShell 来实现这一点：

*   `-PSConsoleFile`，用于加载一个提供待加载管理单元列表的控制台文件。
*   `-Command`，用于指定初始启动命令（随后加载管理单元或模块）
*   `-File`，用于指定初始启动脚本（随后加载管理单元或模块）

无论使用哪一种，您都可以检查生成的已加载扩展集，以查看哪些可以导入到其他 PowerShell 会话中。

### 检测已加载的管理单元

`Get-PsSnapin` 命令返回当前会话中加载的所有管理单元。它始终返回核心 PowerShell 管理单元集，但也会返回自定义环境加载的任何额外管理单元。例如，如果您识别出的管理单元名称为 `Product.Feature.Commands`，则可以通过输入 `Add-PsSnapin Product.Feature.Commands` 将其加载到未来的 PowerShell 会话中。要实现自动化，请将该命令添加到 PowerShell 配置文件中。

如果您不确定要加载哪个管理单元，也可以使用 `Get-Command` 命令来发现哪个管理单元定义了特定命令：

```
PS > Get-Command Get-Counter | Select PsSnapin

PSSnapIn
--------
Microsoft.PowerShell.Diagnostics
```

### 检测已加载的模块

与 `Get-PsSnapin` 命令类似，`Get-Module` 命令返回当前会话中加载的所有模块。它返回您迄今为止添加到该会话中的任何模块，但也会返回自定义环境加载的任何额外模块。例如，如果您识别出的模块名称为 `ProductModule`，则可以通过输入 `Import-Module ProductModule` 将其加载到未来的 PowerShell 会话中。要实现自动化，请将该命令添加到 PowerShell 配置文件中。

如果您不确定要加载哪个模块，也可以使用 `Get-Command` 命令来发现哪个模块定义了特定命令：

```
PS > Get-Command Start-BitsTransfer | Select Module

Module
------
BitsTransfer
```

## 另请参阅

[Recipe 1.28, “使用附加命令扩展 Shell”](#ch01.xhtml#extend_your_shell_with_additional_commands)

# 1.31 在会话之间保存状态

## 问题

您想要在 PowerShell 会话之间保存状态或历史记录。

## 解决方案

订阅 `PowerShell.Exiting` 引擎事件，以便让 PowerShell 调用一个保存所需状态的脚本或脚本块。

## 讨论

PowerShell 提供了基于脚本的简便访问方式，可访问各种系统、引擎和其他事件。您可以注册以接收这些事件的通知，甚至可以自动处理这些事件中的任意一个。在下面的示例中，我们订阅了名为 `PowerShell.Exiting` 的事件。当您关闭会话时，PowerShell 会生成此事件。

您可以使用此事件来保存和恢复状态、变量以及所需的任何其他内容。虽然 `PSReadLine` 模块已经可以自动在会话之间保存命令历史记录，但为了演示目的，我们可以通过 `PowerShell.Exiting` 事件实现类似的功能。您需要在配置文件中放置对 `Enable-HistoryPersistence` 的调用（[示例 1-19](#ch01.xhtml#enable-historypersistence.ps1)）。

### 示例 1-19. Enable-HistoryPersistence.ps1
