
## Search-StartMenu

### 说明

搜索开始菜单中与提供的文本匹配的项目。此脚本同时搜索名称（在开始菜单中显示的名称）和链接的目标。

### 示例

```powershell
PS > Search-StartMenu "Character Map" | Invoke-Item
# 搜索 "Character Map" 应用程序，然后运行它

PS > Search-StartMenu PowerShell | Select-FilteredObject | Invoke-Item
# 搜索应用程序名称中包含 "PowerShell" 的任何内容，让你选择要启动的项目，然后启动它
```

### 参数

```powershell
param(
    ## 要匹配的模式
    [Parameter(Mandatory = $true)]
    $Pattern
)
```

### 脚本内容

```powershell
Set-StrictMode -Version 3

## 获取开始菜单路径的位置
$myStartMenu = [Environment]::GetFolderPath("StartMenu")
$shell = New-Object -Com WScript.Shell
$allStartMenu = $shell.SpecialFolders.Item("AllUsersStartMenu")

## 转义搜索词，以免正则表达式字符影响搜索
$escapedMatch = [Regex]::Escape($pattern)

## 在 "my start menu" 中搜索链接名称或链接目标中的文本
dir $myStartMenu *.lnk -rec | Where-Object {
    ($_.Name -match "$escapedMatch") -or
    ($_ | Select-String "\\[^\\]*$escapedMatch\." -Quiet)
}

## 在 "all start menu" 中搜索链接名称或链接目标中的文本
dir $allStartMenu *.lnk -rec | Where-Object {
    ($_.Name -match "$escapedMatch") -or
    ($_ | Select-String "\\[^\\]*$escapedMatch\." -Quiet)
}
```

### 另请参阅

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

---

# 8.7 程序：显示着色脚本内容

## 讨论

查看或演示脚本时，语法高亮使信息更易于阅读。在 `Visual Studio Code` 中查看脚本是最自然（也是功能最强大）的选择，但你可能也想在控制台中查看它们。

除了基本的语法高亮，脚本审查时其他有用的功能包括行号和高亮显示行范围。在更大的上下文中讨论脚本的部分内容时，范围高亮特别有用。

[示例 8-3](#ch08.xhtml#show-colorizedcontentps1) 通过在控制台会话中提供脚本的语法高亮来实现所有这些场景。[图 8-1](#ch08.xhtml#show_colorized_content_example) 显示了着色内容的示例。

![](Images/wps4_0801.png)

除了本身具有实用性外，`Show-ColorizedContent.ps1` 还演示了如何使用 PowerShell 的 Tokenizer API，该 API 在 [食谱 10.10](#ch10.xhtml#parse_and_interpret_powershell_scripts) 中介绍。虽然此示例中的许多技术专用于 `PowerShell console` 中的语法高亮，但更多技术适用于所有形式的脚本操作。

### 示例 8-3. Show-ColorizedContent.ps1
