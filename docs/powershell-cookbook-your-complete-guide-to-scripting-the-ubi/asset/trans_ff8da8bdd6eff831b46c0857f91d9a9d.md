

# PowerShell 事务支持

PowerShell 的事务支持建立在四个核心 cmdlet 之上：`Start-Transaction`、`Use-Transaction`、`Complete-Transaction` 和 `Undo-Transaction`。

`Start-Transaction` 开始一个事务，创建一个上下文，其中的更改对事务内的命令可见，但对事务外部不可见。在大多数情况下，启动事务后，你可以通过向支持该参数的 cmdlet 添加 `-UseTransaction` 参数，将命令应用到该事务。例如，当 PowerShell 提供程序支持事务时，所有 PowerShell 的核心 cmdlet（`Get-ChildItem`、`Remove-Item` 等）都允许你为针对该提供程序的操作指定 `-UseTransaction` 参数。

`Use-Transaction` cmdlet 略有不同。虽然它仍然需要 `-UseTransaction` 参数将其脚本块应用到当前事务，但其唯一目的是让你能够针对本身支持事务的 .NET 对象进行脚本编写。由于这些对象无法提供 `-UseTransaction` 参数，PowerShell 提供了这个通用的 cmdlet，用于任何类型的事务性 .NET 脚本编写。

## 注意

其他启用事务的 cmdlet 不应在 `Use-Transaction` 脚本块中调用。你仍然需要为被调用的 cmdlet 提供 `-UseTransaction` 参数，并且它们有可能导致 PowerShell 范围内的事务出现不稳定。

为了给用户提供一个比 Windows 注册表风险更小的练习对象，PowerShell 包含了 `Microsoft.PowerShell.Commands.Management.TransactedString` 类。该类的行为正如你对任何事务命令的预期，并让你熟悉 PowerShell 的其余事务 cmdlet 如何协同工作。因为这是一个 .NET 对象，它必须从 `Use-Transaction` cmdlet 的脚本块中调用。

最后，当你完成当前事务的任务时，调用 `Complete-Transaction` 或 `Undo-Transaction` cmdlet。与解决方案相比，这里有一个示例会话，其中 `Undo-Transaction` cmdlet 让你丢弃事务期间所做的更改：

```
PS > Start-Transaction

Suggestion [1,Transactions]: Once a transaction is started, only commands that
get called with the -UseTransaction flag become part of that transaction.
PS >
PS > $transactedString = New-Object Microsoft.PowerShell.Commands.Management.
TransactedString
PS > $transactedString.Append("Hello ")
PS >
PS > Use-Transaction -UseTransaction { $transactedString.Append("World") }

Suggestion [2,Transactions]: The Use-Transaction cmdlet is intended for
scripting of transaction-enabled .NET objects. Its ScriptBlock should contain
nothing else.
PS >
PS > $transactedString.ToString()
Hello
PS >
PS > Undo-Transaction
PS >
PS > $transactedString.ToString()
Hello
```

## 另请参阅

[配方 21.6，“安全地组合相关的注册表修改”](#ch21.xhtml#safely_combine_related_registry_modifications)

## 30.2 在事务中更改错误恢复行为

### 问题

你想要更改 PowerShell 在执行事务 cmdlet 期间遇到错误时的响应方式。

### 解决方案

使用 `Start-Transaction` cmdlet 的 `-RollbackPreference` 参数来控制何种类型的错误会导致 PowerShell 自动撤销你的事务：

```
HKCU:\ > Start-Transaction
HKCU:\ > New-Item Foo -UseTransaction

    Hive: HKEY_CURRENT_USER

SKC  VC Name                           Property
---  -- ----                           --------
  0   0 Foo                            {}

HKCU:\ > Copy IDoNotExist Foo -UseTransaction
Copy-Item : Cannot find path 'HKCU:\IDoNotExist' because it does not exist.

HKCU:\ > Complete-Transaction
Complete-Transaction : Cannot commit transaction. The transaction has been
rolled back or has timed out.

HKCU:\ > Start-Transaction -RollbackPreference TerminatingError

    Hive: HKEY_CURRENT_USER

SKC  VC Name                           Property
---  -- ----                           --------
  0   0 Foo                            {}

HKCU:\ > Copy IDoNotExist Foo -UseTransaction
Copy-Item : Cannot find path 'HKCU:\IDoNotExist' because it does not exist.

HKCU:\ > Complete-Transaction
HKCU:\ > Get-Item Foo

    Hive: HKEY_CURRENT_USER

SKC  VC Name                           Property
---  -- ----                           --------
  0   0 Foo                            {}
```

### 讨论

脚本中的错误是导致系统不一致的极其常见的原因。如果脚本错误地假设注册表项或其他系统状态存在，这种类型的错误往往会贯穿整个脚本。随着脚本继续执行，一些操作成功而另一些失败。当脚本完成时，你将处于困难的境地：不知道脚本的哪些部分正确工作。

有时再次运行脚本会神奇地解决问题。不幸的是，面临繁琐的手动清理工作也同样常见。

解决这些一致性问题是系统事务的主要目标之一。

当 PowerShell 创建一个新事务时，它会为在该事务上下文中运行时遇到的任何错误撤销（回滚）你的事务。当 PowerShell 回滚你的事务时，系统影响是明确的：事务的任何部分都不会成为永久性的，因此你的系统仍然完全一致。

不过，某些情况过于动荡，无法依赖这种严格的一致性解释，因此 PowerShell 在 `Start-Transaction` 上提供了 `-RollbackPreference` 参数，让你配置它应该如何响应错误：

`Error`
: 发生任何错误时，PowerShell 都会回滚你的事务。

`TerminatingError`
: 仅当发生终止错误时，PowerShell 才会回滚你的事务。

`Never`
: PowerShell 永远不会因错误而自动回滚你的事务。

有关 PowerShell 错误处理和错误级别的更多信息，请参阅 [第 15 章](#ch15.xhtml#tracing_and_error_management)。

### 另请参阅

[第 15 章](#ch15.xhtml#tracing_and_error_management)

# 第 31 章 事件处理

## 31.0 简介

系统管理的很大一部分是反应性的：在系统服务关闭时、文件创建或删除时、Windows 注册表发生更改时，甚至在定时间隔到达时采取某些操作。

响应系统更改的最简单方法就是简单地*轮询*它们。如果你在等待文件创建，只需偶尔检查一次直到它出现。如果你在等待进程启动，只需不断调用 `Get-Process` cmdlet 直到它出现。

这种方法对于某些事件（如等待进程来或去）是可以接受的，但当你需要监视系统的大部分内容（如整个注册表或文件系统）时，它很快就会失效。

作为轮询系统更改的替代方案，许多技术支持自动通知——称为*事件*。当应用程序注册这些自动通知时，它可以在事件发生时立即响应，而不必轮询它们。

不幸的是，每种技术都提供自己的事件通知方法：.NET 定义了一种方法，WMI 定义了另一种。当你有一个想要生成自己事件的脚本时，这两种技术都不提供选项。

PowerShell 通过引入一组单一、一致的事件相关 cmdlet 来解决这种复杂性。这些 cmdlet 让你能够处理所有这些不同的事件源。当事件发生时，你可以让 PowerShell 在其事件队列中为你存储通知，或使用 `Action` 脚本块自动处理它。

