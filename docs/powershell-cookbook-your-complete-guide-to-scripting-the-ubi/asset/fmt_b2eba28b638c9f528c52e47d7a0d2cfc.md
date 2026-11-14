
# 创建临时文件

使用 `New-TemporaryFile` cmdlet 创建临时文件：

```
$file = New-TemporaryFile
$file | Set-Content "Some temporary content"
(... use the file ...)
$file | Remove-Item
```

使用 `temp:` PowerShell 驱动器管理已存在的临时文件。查找所有超过 30 天的临时文件：

```
dir temp: | Where-Object LastWriteTime -lt ((Get-Date).AddDays(-30))
```

## Discussion

创建临时文件用于临时用途是很常见的。例如，你可能想要在文件中搜索并替换文本。对大文件执行此操作需要临时文件（参见[Recipe 9.8](#ch09.xhtml#search_and_replace_text_in_a_file)）。另一个例子是 [Recipe 2.4](#ch02.xhtml#program_interactively_filter_lists_of_ob) 中使用的临时文件。

通常，人们会在能想到的任何地方创建临时文件：在 `C:\\`、脚本的当前位置或其他许多地方。虽然这在作者的系统上可能工作，但在其他地方很少能良好运行。例如，如果用户不使用管理员账户进行日常任务，你的脚本将无法访问 `C:\\` 并会失败。

另一个困难来自于尝试为临时文件创建唯一名称。如果脚本只是硬编码一个名称（无论包含多少随机字符），同时运行两个副本时就会失败。你甚至可能编写一个足够智能的脚本来搜索不存在的文件名、创建它并使用它。不幸的是，如果另一个脚本副本在你看到文件不存在后但在你实际创建文件前创建了该文件，这仍然会中断。

最后，如果脚本将临时文件写入其他用户可读或可写的位置，可能会引入多个安全漏洞。

幸运的是，`New-TemporaryFile` cmdlet 为你解决了这些问题。它以可靠且安全的方式在可靠位置创建唯一文件名。该方法返回一个文件对象，你可以按需使用。

> **Note**
>
> 当脚本不再需要该文件时，请记得删除它；否则，脚本会浪费磁盘空间并在用户系统上造成不必要的混乱。记住：你的脚本应该解决管理员的问题，而不是制造问题！

默认情况下，`New-TemporaryFile` cmdlet 返回一个带有 `.tmp` 扩展名的文件。对于大多数用途，文件扩展名无关紧要，这工作良好。在极少数需要创建特定扩展名文件的情况下，使用 `Rename-Item` cmdlet 重命名临时文件。以下示例创建一个使用 `.cs` 扩展名的新临时文件：

```
$file = New-TemporaryFile
$file = $file | Rename-Item -NewName { $_.Name + ".cs" } -PassThru

(... use the file ...)

$file | Remove-Item
```

当你想要在系统通用临时文件位置管理临时文件时，可以使用 `temp:` PowerShell 驱动器。在 Windows 上，这等同于 `$env:TEMP` 位置。在 Linux 机器上，这是 `/tmp`。有关在自定义 PowerShell 驱动器中处理文件的更多信息，请参见 [Recipe 20.15](#ch20.xhtml#create_map_powershell_drive)。

## See Also

- [Recipe 2.4, “Interactively Filter Lists of Objects”](#ch02.xhtml#program_interactively_filter_lists_of_ob)
- [Recipe 9.8, “Search and Replace Text in a File”](#ch09.xhtml#search_and_replace_text_in_a_file)
- [Recipe 20.15, “Create and Map PowerShell Drives”](#ch20.xhtml#create_map_powershell_drive)

# 9.8 Search and Replace Text in a File

## Problem

你想要在文件中搜索文本并将该文本替换为新内容。

## Solution

要在文件中搜索和替换文本，首先将文件内容存储在变量中，然后将替换后的文本存回该文件，如[Example 9-3](#ch09.xhtml#replacing_text_in_a_file)所示。

### Example 9-3. Replacing text in a file

```
PS > $filename = "file.txt"
PS > $match = "source text"
PS > $replacement = "replacement text"
PS >
PS > $content = Get-Content $filename
PS > $content
This is some source text that we want
to replace. One of the things you may need
to be careful about with Source
Text is when it spans multiple lines,
and may have different Source Text
capitalization.
PS >
PS > $content = $content -creplace $match,$replacement
PS > $content
This is some replacement text that we want
to replace. One of the things you may need
to be careful about with Source
Text is when it spans multiple lines,
and may have different Source Text
capitalization.
PS > $content | Set-Content $filename
```

## Discussion

使用 PowerShell 在文件（或多个文件！）中搜索和替换文本是使用工具自动化重复任务的最佳示例之一。手工操作可能需要几个月的时间，但可以缩短到几分钟（最多几小时）。

> **Note**
>
> 请注意，Solution 使用 `-creplace` 运算符以区分大小写的方式替换文本。这几乎总是你想要的，因为替换文本使用你提供的确切大小写。如果要替换的文本以多种不同方式大写（如 Solution 中的 `Source Text`），则使用不同的可能大小写多次搜索和替换。

[Example 9-3](#ch09.xhtml#replacing_text_in_a_file) 说明了也许是最简单（但实际最常见）的场景：

- 你处理 ASCII 文本文件。
- 你用字面文本替换替换某些字面文本。
- 你不担心文本匹配可能跨越多行。
- 你的文本文件相对较小。

如果这些假设中的某些不成立，本讨论将向你展示如何调整在此文件中搜索和替换的方式。

### Work with files encoded in Unicode or another (OEM) code page

默认情况下，`Set-Content` cmdlet 假设你希望输出文件包含纯 ASCII 文本。如果你处理其他编码的文件（例如 Unicode 或 OEM 代码页如 Cyrillic），请使用 `Out-File` cmdlet 的 `-Encoding` 参数指定：

```
$content | Out-File -Encoding Unicode $filename
$content | Out-File -Encoding OEM $filename
```

### Replace text using a pattern instead of plain text

虽然最常见的是用另一个字面字符串替换一个字面字符串，但在某些高级场景中，你可能想要根据模式替换文本。一个例子可能是交换名和姓。PowerShell 通过其替换运算符对正则表达式的支持来支持这种类型的替换：

```
PS > $content = Get-Content names.txt
PS > $content
John Doe
Mary Smith
PS > $content -replace '(.*) (.*)','$2, $1'
Doe, John
Smith, Mary
```

### Replace text that spans multiple lines

Solution 中使用的 `Get-Content` cmdlet 从文件检索行列表。当你对该数组使用 `-replace` 运算符时，它会单独替换每行中的文本。如果你的匹配跨越多行，如 [Example 9-3](#ch09.xhtml#replacing_text_in_a_file) 第 3 行和第 4 行之间所示，`-replace` 运算符将无法感知匹配且不会执行替换。

如果你想要替换跨越多行的文本，则有必要停止将输入文本视为行集合。一旦停止将输入视为行集合，使用能够忽略换行符的替换表达式也很重要，如 [Example 9-4](#ch09.xhtml#replacing_text_across_multiple_lines_in_) 所示。

### Example 9-4. Replacing text across multiple lines in a file

```
$singleLine = Get-Content file.txt -Raw
$content = $singleLine -creplace "(?s)Source(\s*)Text",'Replacement$1Text'
```
