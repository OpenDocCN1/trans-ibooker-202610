
# Show-ColorizedContent

> 来自 *PowerShell Cookbook* (O'Reilly)，作者 Lee Holmes (http://www.leeholmes.com/guide)

## 概要

为 PowerShell 脚本显示语法高亮、行号和范围高亮。

## 示例

**示例 1**

```powershell
PS > Show-ColorizedContent Invoke-MyScript.ps1

001 | function Write-Greeting
002 | {
003 |     param($greeting)
004 |     Write-Host "$greeting World"
005 | }
006 |
007 | Write-Greeting "Hello"
```

**示例 2**

```powershell
PS > Show-ColorizedContent Invoke-MyScript.ps1 -highlightRange (1..3+7)

001 > function Write-Greeting
002 > {
003 >     param($greeting)
004 |     Write-Host "$greeting World"
005 | }
006 |
007 > Write-Greeting "Hello"
```

## 参数

```powershell
param(
    ## 要着色的路径
    [Parameter(Mandatory = $true)]
    $Path,

    ## 要高亮的行范围
    $HighlightRange = @(),

    ## 排除行号的开关
    [Switch] $ExcludeLineNumbers
)
```

## 脚本主体

```powershell
Set-StrictMode -Version 3
```

### 标记颜色

用于不同脚本标记的颜色。要选择自己的颜色：

```powershell
[Enum]::GetValues($host.UI.RawUI.ForegroundColor.GetType()) |
    Foreach-Object { Write-Host -Fore $_ "$_" }
```

```powershell
$replacementColors = @{
    'Attribute' = 'DarkCyan'
    'Command' = 'Blue'
    'CommandArgument' = 'Magenta'
    'CommandParameter' = 'DarkBlue'
    'Comment' = 'DarkGreen'
    'GroupEnd' = 'Black'
    'GroupStart' = 'Black'
    'Keyword' = 'DarkBlue'
    'LineContinuation' = 'Black'
    'LoopLabel' = 'DarkBlue'
    'Member' = 'Black'
    'NewLine' = 'Black'
    'Number' = 'Magenta'
    'Operator' = 'DarkGray'
    'Position' = 'Black'
    'StatementSeparator' = 'Black'
    'String' = 'DarkRed'
    'Type' = 'DarkCyan'
    'Unknown' = 'Black'
    'Variable' = 'Red'
}
```

### 高亮设置

```powershell
$highlightColor = "Red"
$highlightCharacter = ">"
$highlightWidth = 6
if($excludeLineNumbers) { $highlightWidth = 0 }
```

### 读取并标记化文件

```powershell
## 读取文件文本，并对其进行标记化
$content = Get-Content $Path -Raw
$parsed = [System.Management.Automation.PsParser]::Tokenize(
    $content, [ref] $null) | Sort StartLine,StartColumn
```

## 函数

### WriteFormattedLine

写入格式化行 -- 格式为：
`<行号> <分隔符> <文本>`

```powershell
function WriteFormattedLine($formatString, [int] $line)
{
    if($excludeLineNumbers) { return }

    ## 默认以灰色写入行号，并使用简单的竖线作为分隔符
    $hColor = "DarkGray"
    $separator = "|"

    ## 如果需要高亮该行，则使用高亮颜色和高亮分隔符作为分隔符
    if($highlightRange -contains $line)
    {
        $hColor = $highlightColor
        $separator = $highlightCharacter
    }

    ## 写入格式化行
    $text = $formatString -f $line,$separator
    Write-Host -NoNewLine -Fore $hColor -Back White $text
}
```

### CompleteLine

用填充单元完成当前行。

```powershell
function CompleteLine($column)
{
    ## 计算剩余空间
    $lineRemaining = $host.UI.RawUI.WindowSize.Width -
        $column - $highlightWidth + 1

    ## 如果剩余小于 0，说明已换到下一行。再添加一个缓冲区宽度的填充
    if($lineRemaining -lt 0)
    {
        $lineRemaining += $host.UI.RawUI.WindowSize.Width
    }

    Write-Host -NoNewLine -Back White (" " * $lineRemaining)
}
```

## 主处理循环

```powershell
## 写入第一行上下文信息（行号、高亮字符。）
Write-Host
WriteFormattedLine "{0:D3} {1} " 1

## 现在，遍历输入脚本中的每个标记
$column = 1
foreach($token in $parsed)
{
    $color = "Gray"

    ## 通过查找将标记类型映射到其颜色的哈希表，确定该标记的高亮颜色
    $color = $replacementColors[[string]$token.Type]
    if(-not $color) { $color = "Gray" }

    ## 如果是换行标记，写入下一行的上下文信息
    if(($token.Type -eq "NewLine") -or ($token.Type -eq "LineContinuation"))
    {
        CompleteLine $column
        WriteFormattedLine "{0:D3} {1} " ($token.StartLine + 1)
        $column = 1
    }
    else
    {
        ## 处理缩进
        if($column -lt $token.StartColumn)
        {
            $text = " " * ($token.StartColumn - $column)
            Write-Host -Back White -NoNewLine $text
            $column = $token.StartColumn
        }

        ## 查看标记结束位置
        $tokenEnd = $token.Start + $token.Length - 1

        ## 处理多行字符串和注释的行号
        if(
            (($token.Type -eq "String") -or
            ($token.Type -eq "Comment")) -and
            ($token.EndLine -gt $token.StartLine))
        {
            ## 存储起始行
            $lineCounter = $token.StartLine

            ## 将此标记的内容拆分为多行
            ## 我们使用标记的开始和结束来确定内容位置，但使用内容本身（而非标记值）进行操作。
            $stringLines = $(
                -join $content[$token.Start..$tokenEnd] -split "`n")

            ## 遍历内容中的每一行
            foreach($stringLine in $stringLines)
            {
                $stringLine = $stringLine.Trim()

                ## 如果在新行上，用空格填充行右侧，并写入新行的标题
                if($lineCounter -gt $token.StartLine)
                {
                    CompleteLine $column
                    WriteFormattedLine "{0:D3} {1} " $lineCounter
                    $column = 1
                }

                ## 现在写入当前行的文本
                Write-Host -NoNewLine -Fore $color -Back White $stringLine
                $column += $stringLine.Length
                $lineCounter++
            }
        }
        ## 写出常规标记
        else
        {
            ## 我们使用标记的开始和结束来确定内容位置，但使用内容本身（而非标记值）进行操作。
            $text = (-join $content[$token.Start..$tokenEnd])
            Write-Host -NoNewLine -Fore $color -Back White $text
        }

        ## 更新我们在列中的位置
        $column = $token.EndColumn
    }
}

CompleteLine $column
Write-Host
```

## 另请参阅

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [食谱 10.10，“解析和解释 PowerShell 脚本”](#ch10.xhtml#parse_and_interpret_powershell_scripts)

---

# 第三部分. 常见任务

- [第 9 章，*简单文件*](#ch09.xhtml#simple_files)
- [第 10 章，*结构化文件*](#ch10.xhtml#structured_files)
- [第 11 章，*代码重用*](#ch11.xhtml#code_reuse)
- [第 12 章，*联网脚本*](#ch12.xhtml#internet_enabled_scripts)
- [第 13 章，*用户交互*](#ch13.xhtml#user_interaction)
- [第 14 章，*调试*](#ch14.xhtml#chapter_debugging)
- [第 15 章，*跟踪和错误管理*](#ch15.xhtml#tracing_and_error_management)
- [第 16 章，*环境感知*](#ch16.xhtml#environmental_awareness)
- [第 17 章，*扩展 PowerShell 的影响力*](#ch17.xhtml#extend_the_reach_of_windows_powershell)
- [第 18 章，*安全性和脚本签名*](#ch18.xhtml#security_and_script_signing)
- [第 19 章，*Visual Studio Code*](#ch19.xhtml#visual_studio_code)

# 第 9 章. 简单文件

## 9.0 简介
