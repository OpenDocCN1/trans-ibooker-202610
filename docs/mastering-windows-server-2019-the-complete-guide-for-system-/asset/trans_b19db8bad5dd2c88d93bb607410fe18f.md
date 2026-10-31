
将新的 WEB4 服务器添加到我们的受管机器列表中，现在可以从此实例的 Server Manager 中进行管理。回到我最初的意图，我想在 WEB4 上安装 Web Server (IIS) 角色。如果我在 Server Manager 中使用“添加角色和功能”功能，现在就可以选择操作 WEB4 服务器：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_446_0.png)

图 10.18：从 WEB3 的 Server Manager 修改 WEB4

与运行 Windows Server 完整桌面体验版本的任何服务器一样，我们现在可以完成角色安装向导的步骤，Web Server 角色将被安装在 WEB4 上。

## 远程服务器管理工具

同样重要的是，你可以使用 Windows 10 中的远程服务器管理工具 (RSAT) 来管理 Server Core 实例。RSAT 本质上只是 Server Manager 以及配套管理工具的副本，旨在在客户端操作系统上运行。在我们的例子中，我已经有一台 Windows 10 机器，书中前面已经在上面安装了 RSAT，所以我将通过登录该机器并将 WEB4 添加到界面中来进行测试。我刚刚在之前的任务中在 WEB4 上安装了 IIS 角色，所以当我将它连接到 WEB4 时，应该能在 RSAT 中看到该列表。

如果你之前没有使用过 RSAT，也没有阅读过我们文本中该部分的内容，重要的是要知道并没有一个名为“远程服务器管理工具”的应用程序。相反，在完成 RSAT 安装后，请在“开始”菜单中查找名为 Server Manager 的应用程序。这就是如何利用 Windows 10 客户端远程管理 Windows Server 2019 实例的方式：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_447_0.png)

图 10.19：Windows 10 中的 Server Manager，借助 RSAT

正如你在 Windows Server 2019 的 Server Manager 界面中所做的那样，继续通过向导添加其他要管理的服务器。一旦我在我的 Win10 的 Server Manager 中将 WEB4 添加为受管服务器，我就可以在我的仪表板中看到 IIS 的列表。这表明运行在 WEB4 上的 IIS 服务可以直接从我的 Windows 10 桌面计算机中看到、访问和配置。对于我在 WEB4 上需要完成的大多数任务，我永远不必担心登录该服务器的控制台。

如果我在该 RSAT 控制台中右键单击 WEB4 服务器名称，你可以看到我有许多可用的功能，可以用来管理此远程 Server Core 实例：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_447_1.png)

图 10.20 通过 Server Manager 与远程服务器交互

因此，你可以看到有方法可以使用 GUI 工具来管理我们的无 GUI Windows Server 实例。这只是将思维放在把服务器视为无头服务器的位置上，而 PowerShell 或 Server Manager 等工具根本不在乎它们所更改的服务器是本地还是远程。流程和工具在任何情况下都相同。你可以在之前的屏幕截图中看到，我甚至可以从这里单击启动到 WEB4 的远程 PowerShell 连接。单击此按钮会立即启动一个 PowerShell 提示，该提示远程连接到 WEB4 服务器，即使我目前仅登录到我的 Windows 10 工作站。这甚至比在 PowerShell 内部发出 Enter-PSSession cmdlet 更容易。

## 意外关闭命令提示符

让我们现在直接从 Server Core 控制台来看一件事情；如果你没有经常使用 Server Core，这是一个常见的障碍。我们倾向于关闭不再使用的窗口和应用程序，因此你可能会无意识地关闭在 Server Core 控制台会话中提供你整个管理存在的命令提示符窗口。现在你坐在一个大空白屏幕前，似乎没有任何界面，也无处可去。

你如何在这个服务器上恢复工作？我们是否必须关闭服务器然后重新打开以重置它？这会中断该服务器可能为用户提供的任何角色或流量，因此显然不是理想的方法。

有一个简单的方法可以恢复命令提示符，那就是使用任务管理器启动一个新的命令提示符实例。在错误关闭当前命令提示符窗口后，当你坐在 Server Core 控制台的空黑色屏幕前时，你可以按 Ctrl + Alt + Del，然后会出现以下选项：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_448_0.png)

图 10.21：Ctrl+Alt+Del 在 Server Core 中有效

这里可以执行一些不同的功能，这很不错。但要恢复我们的命令提示符窗口，请向下箭头到 **任务管理器** 并按 **Enter**。这将启动我们都熟悉的 **任务管理器** 应用程序。现在，单击 **显示更多详细信息** 以展开任务管理器的屏幕。展开 **文件** 菜单，然后单击 **运行新任务**：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_449_0.png)

图 10.22：通过任务管理器运行新任务

在 **创建新任务** 框中，输入 `cmd` 然后单击 **确定**：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_449_1.png)

图 10.23：打开命令提示符

另外，你也可以直接从此 **创建新任务** 提示中指定启动任何应用程序。如果你有兴趣直接进入 PowerShell，而不是输入 cmd，你可以简单地在该提示中输入 powershell，它就会直接打开：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_450_0.png)

图 10.24：在 Server Core 中打开 PowerShell

## 使用 Windows Admin Center 管理 Server Core

虽然来自控制台的命令提示符、远程 PowerShell 连接、远程 Server Manager 管理，甚至在 Windows 10 工作站上运行的 RSAT 工具都是管理我们的 Server Core 实例的有效且强大的工具，但它们现在都已被 Windows Admin Center 的发布所取代。你已经了解 Windows Admin Center 可以为集中管理整个服务器基础设施做什么，但我们需要在此指出的是，WAC 可用于具有和不具有图形界面的服务器。

我与许多 Windows Server 管理员讨论过 Server Core 的话题，实现这些更高效、更安全的服务器平台的最大障碍之一是担心，一旦配置完成，这些服务器的持续管理和维护将更难处理。熟悉并习惯于在 Windows Server 桌面体验中工作的管理员确切知道需要做什么来完成日常任务，但移除那种点按式界面后，工作日突然变得复杂得多。

值得庆幸的是，使用 Server Core 你不必记住 PowerShell 手册！Windows Admin Center 将 Server Core 实例视为与运行桌面体验的服务器相同的方式。它就是能工作！

我们已经在测试实验室的一台服务器上安装了 WAC，所以让我们打开它并添加我的新 WEB4 服务器以供管理，并看看可用于此服务器持续维护的选项。

当我们首次通过 WAC 控制台连接到 WEB4 时，这里没有任何内容表明这是一个 Server Core 实例。我们有所有可供点击的 WAC 工具和实用程序：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_451_0.png)

图 10.25：WAC 概览 WEB4

让我们从 Windows Admin Center 尝试几件事。你在屏幕顶部附近有电源控制，可以轻松关闭或重新启动服务器。这比必须建立远程 PowerShell 连接然后发出命令来完成相同操作要容易和快速得多。主屏幕上（如果向下滚动）还有性能指标，显示你消耗的 CPU、Memory 和 Networking 资源。如果没有 WAC，你需要登录 WEB4 并启动任务管理器才能看到这些统计信息：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_452_0.png)

图 10.26：服务器性能指标
