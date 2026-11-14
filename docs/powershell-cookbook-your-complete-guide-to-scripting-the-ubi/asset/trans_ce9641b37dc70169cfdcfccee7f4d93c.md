
您可以随意组合这些筛选器。例如，要查找所有为 `ReadOnly` 或 `Hidden` 但非 `System` 的项目：

```
Get-ChildItem c:\ -Attributes "ReadOnly, Hidden + !System"
```

在文件系统中，这些 cmdlet 返回来自 .NET Framework 的对象，这些对象分别代表文件和目录——即 `System.IO.FileInfo` 和 `System.IO.DirectoryInfo` 类的实例。每个对象都提供大量有用信息：属性、修改时间、全名等。虽然默认目录列表显示了大量信息，但 PowerShell 提供的信息更多。有关使用 .NET Framework 类的详细信息，请参阅 [食谱 3.8](#ch03.xhtml#work_with_net_objects)。

## 另请参阅

[食谱 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

[食谱 20.6，“查找匹配模式的文件”](#ch20.xhtml#find_files_that_match_a_pattern)

# 20.3 查找在特定日期之前修改的所有文件

## 问题

您想要查找在特定日期之前最后修改的所有文件。

## 解决方案

要查找在特定日期之前修改的所有文件，请使用 `Get-ChildItem` cmdlet 列出目录中的文件，然后使用 `Where-Object` cmdlet 将 `LastWriteTime` 属性与您感兴趣的日期进行比较。例如，要查找 2007 年之前创建的所有文件：

```
Get-ChildItem -Recurse | Where-Object { $_.LastWriteTime -lt "01/01/2007" }
```

## 讨论

将文件与特定日期进行比较的常见原因是查找最近修改过（或未最近修改过）的文件。此处的代码与解决方案中给出的示例几乎相同，不同之处在于您的脚本无法知道要比较的确切日期。

在这种情况下，.NET Framework 的 `DateTime` 类中的 `AddDays()` 方法为您提供了一种执行简单日历运算的方法。如果您有一个 `DateTime` 对象，可以对其添加或减去时间以表示完全不同的日期。例如，要查找过去 30 天内修改的所有文件：

```
$compareDate = (Get-Date).AddDays(-30)
Get-ChildItem -Recurse | Where-Object { $_.LastWriteTime -ge $compareDate }
```

同样，要查找超过 30 天的所有文件：

```
$compareDate = (Get-Date).AddDays(-30)
Get-ChildItem -Recurse | Where-Object { $_.LastWriteTime -lt $compareDate }
```

在本示例中，`Get-Date` cmdlet 返回一个表示当前日期和时间的对象。您调用 `AddDays()` 方法从该时间中减去 30 天，将代表“30 天前”的日期存储在 `$compareDate` 变量中。接下来，您将该日期与 `Get-ChildItem` cmdlet 返回的每个文件的 `LastWriteTime` 属性进行比较。

### 注意

`DateTime` 类是管理员最喜爱的日历！

```
PS > [DateTime]::IsLeapYear(2008)
True
PS > $daysTillChristmas = [DateTime] "December 25" - (Get-Date)
PS > $daysTillChristmas.Days
327
```

有关 `Get-ChildItem` cmdlet 的详细信息，请输入 `Get-Help Get-ChildItem`。有关 `Where-Object` cmdlet 的详细信息，请参阅 [食谱 2.2](#ch02.xhtml#filter_items_in_a_list_or_command_output)。

## 另请参阅

[食谱 2.2，“筛选列表或命令输出中的项目”](#ch02.xhtml#filter_items_in_a_list_or_command_output)

# 20.4 清除文件内容

## 问题

您想要清除文件的内容。

## 解决方案

要清除文件的内容，请使用 `Clear-Content` cmdlet，如 [示例 20-1](#ch20.xhtml#clearing_content_from_a_file) 所示。

### 示例 20-1. 从文件中清除内容

```
PS > Get-Content test.txt
Hello World
PS > Clear-Content test.txt
PS > Get-Content test.txt
PS > Get-Item test.txt

    Directory: C:\temp

Mode   LastWriteTime        Length  Name
----   -------------        ------  ----
-a---  4/23/2007   8:05 PM  0       test.txt
```

## 讨论

（名副其实的）`Clear-Content` cmdlet 会清除项目的内容。虽然解决方案仅演示了文件系统中的文件，但它实际上适用于支持“内容”概念的任何 PowerShell 提供程序。支持这些内容概念的其他驱动器示例包括 `Function:`、`Alias:` 和 `Variable:`。

有关如何完全删除项目的信息，请参阅 [食谱 20.12](#ch20.xhtml#remove_a_file_or_directory)。

有关 `Remove-Item` 或 `Clear-Content` cmdlet 的详细信息，请输入 `Get-Help Remove-Item` 或 `Get-Help Clear-Content`。

## 另请参阅

[食谱 20.12，“删除文件或目录”](#ch20.xhtml#remove_a_file_or_directory)

# 20.5 管理和更改文件属性

## 问题

您想要更新文件的 `ReadOnly`、`Hidden` 或 `System` 属性。

## 解决方案

大多数情况下，您会想要使用熟悉的 `attrib.exe` 程序来更改文件的属性：

```
attrib +r test.txt
attrib -s test.txt
```

要仅设置 `ReadOnly` 属性，您可以选择在文件上设置 `IsReadOnly` 属性：

```
$file = Get-Item test.txt
$file.IsReadOnly = $true
```

要应用一组特定的属性，请使用文件上的 `Attributes` 属性：

```
$file = Get-Item test.txt
$file.Attributes = "ReadOnly,NotContentIndexed"
```

目录列表显示文件的属性，但您也可以直接访问 `Mode` 或 `Attributes` 属性：

```
PS > $file.Attributes = "ReadOnly","System","NotContentIndexed"
PS > $file.Mode
--r-s
PS > $file.Attributes
ReadOnly, System, NotContentIndexed
```

## 讨论

当 `Get-Item` 或 `Get-ChildItem` cmdlet 检索文件时，生成的输出具有 `Attributes` 属性。除了常规的 `attrib.exe` 程序外，此属性并没有提供太多额外功能，尽管它确实使将属性设置为特定状态变得更加容易。

### 注意

请注意，在文件上设置 `Hidden` 属性会使其从大多数默认视图中移除。如果您想在隐藏它后检索它，大多数命令都需要 `-Force` 参数。同样，在文件上设置 `ReadOnly` 属性会导致对该文件的大多数写入操作失败，除非您使用 `-Force` 参数调用该命令。

如果您想使用 `Attributes` 属性（出于某种原因而非 `attrib.exe`）向文件添加属性，操作方法如下：

```
$file = Get-Item test.txt
$readOnly = [IO.FileAttributes] "ReadOnly"
$file.Attributes = $file.Attributes -bor $readOnly
```

有关使用 .NET Framework 类的详细信息，请参阅 [食谱 3.8](#ch03.xhtml#work_with_net_objects)。

## 另请参阅

[食谱 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

# 20.6 查找匹配模式的文件

## 问题

您想要获取匹配特定模式的文件列表。

## 解决方案

使用 `Get-ChildItem` cmdlet 以获得简单和高级通配符支持：

*   要查找当前目录中与 PowerShell 通配符匹配的所有项目，请将该通配符提供给 `Get-ChildItem` cmdlet：

    ```
    Get-ChildItem *.txt
    ```

*   要查找当前目录中与*提供程序特定*筛选器匹配的所有项目，请将该筛选器提供给 `-Filter` 参数：

    ```
    Get-ChildItem -Filter *~2*
    ```

*   要查找当前目录中与 PowerShell 通配符**不**匹配的所有项目，请将该通配符提供给 `-Exclude` 参数：

    ```
    Get-ChildItem -Exclude *.txt
    ```

*   要查找子目录中与 PowerShell 通配符匹配的所有项目，请使用 `-Include` 和 `-Recurse` 参数，或将通配符作为 `-Path` 参数的一部分：

    ```
    Get-ChildItem -Include *.txt -Recurse
    Get-ChildItem *.txt -Recurse
    Get-ChildItem -Path c:\temp\*.txt -Recurse
    ```

*   要查找子目录中与*提供程序特定*筛选器匹配的所有项目，请使用 `-Filter` 和 `-Recurse` 参数：

    ```
    Get-ChildItem -Filter *.txt -Recurse
    ```

*   要查找子目录中与 PowerShell 通配符**不**匹配的所有项目，请使用 `-Exclude` 和 `-Recurse` 参数：
