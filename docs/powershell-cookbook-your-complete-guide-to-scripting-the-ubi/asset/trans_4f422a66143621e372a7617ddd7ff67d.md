
## PowerShell Move-Item 帮助信息

```
PS > Get-Help Move-Item -Full
(...)
PARAMETERS

    -path <string[]>
        Specifies the path to the current location of the items. The default
        is the current directory. Wildcards are permitted.

        Required?                    true
        Position?                    1
        Default value                <current location>
        Accept pipeline input?       true (ByValue, ByPropertyName)
        Accept wildcard characters?  true

    -destination <string>
        Specifies the path to the location where the items are being moved.
        The default is the current directory. Wildcards are permitted, but
        the result must specify a single location.

        To rename the item being moved, specify a new name in the value of
        Destination.

        Required?                    false
        Position?                    2
        Default value                <current location>
        Accept pipeline input?       true (ByPropertyName)
        Accept wildcard characters?  True
        (...)
```

如果你有意将 CSV 中的列命名为与接受管道按属性名传值的参数相对应，PowerShell 可以为你完成部分（或全部）参数映射。[示例 2-8](#ch02.xhtml#using_the_import-csv_cmdlet_to_automate_) 演示了一个批量移动项目的 CSV 文件。

### 示例 2-8. 使用 `Import-Csv` cmdlet 自动化接受管道按属性名传值的 cmdlet

```
PS > Get-Content ItemMoves.csv
Path,Destination
test.txt,Test1Directory
test2.txt,Test2Directory

PS > dir test.txt,test2.txt | Select Name

Name
----
test.txt
test2.txt

PS > Import-Csv ItemMoves.csv | Move-Item
PS > dir Test1Directory | Select Name

Name
----
test.txt

PS > dir Test2Directory | Select Name

Name
----
test2.txt
```

有关 `ForEach-Object` cmdlet 和 `foreach` 脚本关键字的更多信息，请参阅[配方 2.5](#ch02.xhtml#work_with_each_item_in_a_list_or_command)。有关处理 CSV 文件的更多信息，请参阅[配方 10.7](#ch10.xhtml#import_structured_data_from_a_csv_file)。有关使用 WMI 的更多信息，请参阅[第 28 章](#ch28.xhtml#windows_management_instrumentation)。

## 另请参阅

- [配方 2.5，“处理列表或命令输出中的每个项目”](#ch02.xhtml#work_with_each_item_in_a_list_or_command)
- [配方 10.7，“从文件导入 CSV 和分隔数据”](#ch10.xhtml#import_structured_data_from_a_csv_file)
- [第 28 章](#ch28.xhtml#windows_management_instrumentation)

## 2.7 拦截管道阶段

### 问题

你想要在 PowerShell 管道的不同阶段拦截或执行某些操作。

### 解决方案

使用[配方 11.23](#ch11.xhtml#enhance_extend_existing_cmdlet) 中提供的 `New-CommandWrapper` 脚本来包装 `Out-Default` 命令，并将你的自定义功能放入其中。

### 讨论

对于任何管道，PowerShell 都会在末尾添加一个对 `Out-Default` cmdlet 的隐式调用。通过在此函数上添加命令包装器，我们可以大量自定义管道处理行为。

当 PowerShell 创建管道时，它首先调用管道中每个命令的 `BeginProcessing()` 方法。对于高级函数（由 `New-CommandWrapper` 脚本创建的类型），PowerShell 会调用 `Begin` 块。如果你想在管道开始时执行任何操作，请将自定义代码放在该块中。

对于管道发出的每个对象，PowerShell 将该对象发送给管道中下一个命令的 `ProcessRecord()` 方法。对于高级函数（由 `New-CommandWrapper` 脚本创建的类型），PowerShell 会调用 `Process` 块。如果你想为管道中的每个元素执行任何操作，请将自定义代码放在该块中。

最后，当 PowerShell 处理完管道中的所有项目时，它会调用管道中每个命令的 `EndProcessing()` 方法。对于高级函数（由 `New-CommandWrapper` 脚本创建的类型），PowerShell 会调用 `End` 块。如果你想在管道结束时执行任何操作，请将自定义代码放在该块中。

有关此方法的两个示例，请参阅[配方 2.8](#ch02.xhtml#capture-pipeline-objects) 和[配方 11.22](#ch11.xhtml#invoke-dynamically-named-commands)。

有关运行脚本的更多信息，请参阅[配方 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

### 另请参阅

- [配方 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [配方 2.8，“自动捕获管道输出”](#ch02.xhtml#capture-pipeline-objects)
- [配方 11.22，“调用动态命名的命令”](#ch11.xhtml#invoke-dynamically-named-commands)
- [配方 11.23，“程序：增强或扩展现有 Cmdlet”](#ch11.xhtml#enhance_extend_existing_cmdlet)

## 2.8 自动捕获管道输出

### 问题

你想要自动捕获最后一条命令的输出，而无需显式将其输出存储在变量中。

### 解决方案

使用 `$PSDefaultParameterValues` 自动变量将 `Out-Default` 命令的 `-OutVariable` 参数值设置为你选择的变量名：

```
$PSDefaultParameterValues["Out-Default:OutVariable"] = "__"
```

### 讨论

一旦命令中的每个对象都通过了管道，它最终会到达末端。如果你的脚本没有捕获此输出，PowerShell 会将其提供给 `Out-Default` cmdlet，后者负责确定如何格式化和显示输出。

与所有 cmdlet 一样，`Out-Default` cmdlet 支持 `-OutVariable` 参数，允许你将其输出存储到变量中：

```
PS > 1..3 | Out-Default -OutVariable myOutput
1
2
3

PS > $myOutput
1
2
3
```

了解这一点后，我们可以利用 PowerShell 的 `$PSDefaultParameterValues` 基础设施，让 `Out-Default` 每次都执行此操作。解决方案使用两个下划线字符作为变量名，以类似于 PowerShell 中表示当前管道输入的单下划线，但你可以使用任何想要的变量名：

```
PS > $PSDefaultParameterValues["Out-Default:OutVariable"] = "lastOutput"

PS > 1..3
1
2
3

PS > $lastOutput
1
2
3
```

有关为 cmdlet 参数提供默认值的更多信息，请参阅[配方 1.5](#ch01.xhtml#default_parameter_values)。

### 另请参阅

- [配方 1.5，“为参数提供默认值”](#ch01.xhtml#default_parameter_values)
- [配方 2.7，“拦截 PowerShell 管道阶段”](#ch02.xhtml#intercept-powershell-pipeline)
- [配方 11.23，“程序：增强或扩展现有 Cmdlet”](#ch11.xhtml#enhance_extend_existing_cmdlet)

## 2.9 捕获和重定向二进制进程输出

### 问题

你想要运行在它们之间传输复杂二进制数据的程序。

### 解决方案

使用 `Invoke-BinaryProcess` 脚本来调用该程序，如[示例 2-9](#ch02.xhtml#invoke-binaryprocess.ps1) 所示。如果它是二进制数据的来源，请使用 `-RedirectOutput` 参数。如果它消费二进制数据，请使用 `-RedirectInput` 参数。

### 示例 2-9. Invoke-BinaryProcess.ps1
