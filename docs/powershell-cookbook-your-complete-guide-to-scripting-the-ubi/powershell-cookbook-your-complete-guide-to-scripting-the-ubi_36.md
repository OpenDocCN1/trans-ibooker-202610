# 第 30 章 事务

## 30.0 简介

*事务* 描述了系统支持暂时性或多步骤更改的能力。当你在事务上下文中进行更改时，系统提供四大保证：

**隔离性**

: 对于未参与事务的观察者而言，事务内的命令尚未影响系统。

**原子性**

: 一旦你决定完成（*提交*）事务，要么所有更改生效，要么全不生效。

**一致性**

: 事务期间导致系统状态不一致的错误会被处理，以使系统恢复到一致状态。

**持久性**

: 系统通知你事务成功完成后，你可以确定这些更改是永久的。

作为事务的现实例子，考虑两个银行账户之间的转账。这可能分两阶段进行：从第一个账户扣款，然后向第二个账户存款。在这种情况下，你对健壮性和正确性有完全相同的目标：

**隔离性**

: 在转账进行中（但尚未完成）时，两个银行账户的余额看起来保持不变。

**原子性**

: 在过程中的某个时刻，可能已经从第一个账户扣款但尚未存入第二个账户。处理转账时，系统绝不能显示这种中间状态。要么所有更改生效，要么全不生效。

**一致性**

: 如果转账期间发生错误，系统会采取纠正措施，确保不会留在中间状态。例如，它可能通过添加透支费或完全放弃转账来处理资金不足的情况。它绝不应该例如从一个账户扣款而不存入第二个账户。

**持久性**

: 转账完成后，你不必担心系统错误撤销全部或部分操作。

虽然事务通常是开发人员的话题，但 PowerShell 将事务作为最终用户概念公开，为一致的系统管理打开了巨大潜力。

要启动事务，请调用 `Start-Transaction` cmdlet。要使用支持事务的 cmdlet，请指定 `-UseTransaction` 参数。显式指定此参数至关重要，因为许多支持事务的 cmdlet 在没有事务的情况下也能同样良好地工作。因此，只有当你提供此参数时，PowerShell 才允许 cmdlet 参与事务。

PowerShell 的注册表提供程序将事务作为一等概念支持。你可以在 配方 21.6 中看到其实际操作。

```
PS > Set-Location HKCU:
PS > Start-Transaction

PS > mkdir TempKey -UseTransaction

    Hive: HKEY_CURRENT_USER

SKC  VC Name                           Property
---  -- ----                           --------
  0   0 TempKey                        {}

PS > New-Item TempKey\TempKey2 -UseTransaction


    Hive: HKEY_CURRENT_USER\TempKey
SKC  VC Name                           Property
---  -- ----                           --------
  0   0 TempKey2                       {}

PS > Get-ChildItem TempKey
Get-ChildItem : Cannot find path 'HKEY_CURRENT_USER\TempKey' because it
does not exist.

PS > Complete-Transaction
PS > Get-ChildItem TempKey

    Hive: HKEY_CURRENT_USER\TempKey

SKC  VC Name                           Property
---  -- ----                           --------
  0   0 TempKey2                       {}
```

完成事务工作后，调用 `Complete-Transaction` cmdlet 使其生效，或调用 `Undo-Transaction` cmdlet 丢弃更改。虽然你现在可能想在其他提供程序（例如文件系统）上尝试事务，但请注意，目前只有注册表提供程序支持它们。

## 30.1 安全地试验事务

### 问题

你想试验 PowerShell 的事务支持，但不想使用注册表提供程序作为试验场。

### 解决方案

使用 PowerShell 的 `System.Management.Automation.TransactedString` 对象以及 `Use-Transaction` cmdlet 来试验字符串，而不是注册表项：

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
PS > Complete-Transaction
PS >
PS > $transactedString.ToString()
Hello World
PS >
```

### 讨论




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

配方 21.6，“安全地组合相关的注册表修改”

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

有关 PowerShell 错误处理和错误级别的更多信息，请参阅 第 15 章。

### 另请参阅

第 15 章
