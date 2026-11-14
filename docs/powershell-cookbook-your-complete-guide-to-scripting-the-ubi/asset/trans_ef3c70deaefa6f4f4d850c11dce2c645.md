

```powershell
PS > function HostWriter { Write-Host "Console Output" }
PS > $a = HostWriter
Console Output
PS > $a
PS > $a = HostWriter 6>&1
PS > $a
Console Output
```

# 常见自定义点

尽管开箱即用非常有用，但 PowerShell 还提供了多种自定义和个性化的途径。

## 控制台设置

Windows PowerShell UI 提供了多项功能，使您的 Shell 体验更加高效。

### 调整字体大小

Windows Terminal 应用程序和默认的 Windows 控制台都允许您调整字体大小。

要临时更改字体大小，请按住 Ctrl 键并使用鼠标向上或向下滚动。在 Windows Terminal 应用程序中，您也可以使用 Ctrl+Plus 或 Ctrl+Minus 热键。在 Windows Terminal 应用程序中，Ctrl+0 可将字体大小重置回默认值。

要在默认的 Windows 控制台中更改默认字体大小，请打开系统菜单（右键单击控制台窗口左上角的标题栏），选择“属性”→“字体”。如果从“开始”菜单启动 Windows PowerShell，它将以对字体和窗口大小的一些默认修改启动。要在 Windows Terminal 应用程序中更改默认字体大小，请向任意终端配置文件添加 `fontSize` 设置：

```json
{
    "guid": "...",
    "name": "PowerShell (Demos)",
    "fontSize": 18,
    "colorScheme": "Campbell Powershell",
    "source": "Windows.Terminal.PowershellCore"
}
```

### 调整其他 Windows Terminal 设置

Windows Terminal 应用程序包含大量配置设置。其中包括：

*   配置可用 Shell 和应用程序的列表（如 `bash.exe`）
*   配色方案和 UI 主题
*   将操作绑定到热键
*   文本选择行为
*   窗口透明度
*   背景图像

有关这些设置的完整列表，请参阅 Windows Terminal 中 [全局设置](https://aka.ms/terminal-global-settings) 和 [常规配置文件设置](https://aka.ms/terminal-profile-settings) 的文档。

### 使用热键更高效地操作 Shell

PowerShell 控制台支持许多热键，有助于提高控制台操作效率，如 **表 A-17** 所示。

**表 A-17. PowerShell 热键**

| 热键 | 含义 |
| --- | --- |
| 按下并释放 Windows 键，然后输入 `pwsh` 或 `powershell` | 启动 PowerShell 或 Windows PowerShell。`Win+X` 热键也提供了一种快速启动 Windows PowerShell 的方式。 |
| 上箭头 | 向后浏览命令历史记录。 |
| 下箭头 | 向前浏览命令历史记录。 |
| 左箭头 | 将光标在命令行上向左移动一个字符。 |
| 右箭头 | 将光标在命令行上向右移动一个字符。如果在行尾，则在该位置插入上一条命令文本中的一个字符。 |
| Ctrl+左箭头 | 将光标在命令行上向左移动一个单词。 |
| Ctrl+右箭头 | 将光标在命令行上向右移动一个单词。 |
| Home 键 | 将光标移至命令行开头。 |
| End 键 | 将光标移至命令行末尾。 |
| Ctrl+Shift+PgUp, Ctrl+Shift+PgDn | 在 Windows Terminal 应用程序中，滚动屏幕缓冲区。在 Windows 控制台中，可使用 PgUp 和 PgDn。 |
| Ctrl+Shift+F | 在 Windows Terminal 应用程序中，搜索屏幕缓冲区中的文本。在 Windows 控制台中，可使用 Alt+Space E F。 |
| Alt+Space E K | 在 Windows 控制台中，选择要从屏幕缓冲区复制的文本。有关另一种方法，请参阅 [配方 1.10](#ch01.xhtml#customize_user_input)。 |
| Ctrl+C | 取消当前操作。如果选中了文本，Ctrl+C 会将该文本复制到剪贴板。 |
| Ctrl+V | 粘贴剪贴板内容。 |
| Ctrl+Shift+T | 在 Windows Terminal 应用程序中，打开新标签页。也可使用 Ctrl+Shift+1、Ctrl+Shift+2 等打开对应编号的配置文件标签页（如 `bash.exe`）。 |
| Ctrl+Shift+W, Alt+F4 | 在 Windows Terminal 应用程序中，关闭当前标签页或整个应用程序。在 Windows 控制台中，可使用 Alt+Space C 关闭整个应用程序。 |
| Ctrl+Break | 在 Windows 控制台中，中断 PowerShell 调试器进入当前正在运行的脚本。 |
| Ctrl+Home | 删除从当前命令行开头到（但不包括）当前光标位置的字符。 |
| Ctrl+End | 删除从当前光标位置（包括该位置）到当前命令行末尾的字符。 |
| Ctrl+Z, Ctrl+Y | 撤销和重做。 |
| F8 | 向后浏览命令历史，仅显示与命令行上已输入文本匹配的命令。 |
| Ctrl+R | 根据交互式输入的文本，开始在命令历史中交互式地向后搜索。 |

> **注意**
>
> PowerShell 通过 `PSReadLine` 模块提供的命令行编辑体验比此表列出的要丰富得多。它包含 Emacs 和 Vi 按键绑定，以及定义自己的功能——您可以通过输入 `Get-PSReadLineKeyHandler` 查看完整的默认列表。有关详细信息，请参阅 [配方 1.10](#ch01.xhtml#customize_user_input)。

## 配置文件

PowerShell 在启动期间会自动运行 **表 A-18** 中列出的四个脚本。如果存在，每个脚本都允许您自定义执行环境。PowerShell 运行您放入这些文件中的任何内容，就如同您在命令行中手动输入一样。

**表 A-18. PowerShell 配置文件**

| 配置文件用途 | 配置文件位置 |
| --- | --- |
| 自定义所有 PowerShell 会话，包括系统上所有用户的 PowerShell 宿主应用程序 | `InstallationDirectory\profile.ps1` |
| 自定义系统上所有用户的 `pwsh.exe` 会话 | `InstallationDirectory\Microsoft.PowerShell_profile.ps1` |
| 自定义所有 PowerShell 会话，包括 PowerShell 宿主应用程序 | `<My Documents>\PowerShell\profile.ps1` |
| `pwsh.exe` 会话的典型自定义 | `<My Documents>\PowerShell\Microsoft.PowerShell_profile.ps1` |

在 Windows PowerShell 中，其中一些位置会有所不同。

PowerShell 通过定义自动变量 `$profile` 使编辑配置文件脚本变得简单。单独使用时，它指向“当前用户、 `pwsh.exe`”配置文件。此外，`$profile` 变量定义了指向其他配置文件位置的附加属性：

```powershell
PS > $profile | Format-List -Force

AllUsersAllHosts       : C:\...Microsoft.PowerShell...\profile.ps1
AllUsersCurrentHost    : C:\...Microsoft.PowerShell...\Microsoft.PowerShell_profile.ps1
CurrentUserAllHosts    : D:\Lee\PowerShell\profile.ps1
CurrentUserCurrentHost : D:\Lee\PowerShell\Microsoft.PowerShell_profile.ps1
```

要创建新配置文件，请输入：

```powershell
New-Item -Type file -Force $profile
```

要编辑此配置文件，请输入：

```powershell
notepad $profile
```

## 提示符

要自定义提示符，请将 `prompt` 函数添加到您的配置文件中。此函数返回一个字符串。例如：

```powershell
function prompt
{
    "PS [$env:COMPUTERNAME] >"
}
```

有关自定义提示符的详细信息，另请参阅 [配方 1.9](#ch01.xhtml#customize_your_shell_profile_and_prompt)。

## 制表符补全

您可以定义 `TabExpansion2` 函数来自定义按 Tab 键时 PowerShell 补全属性、变量、参数和文件的方式。

不过，您的 `TabExpansion` 函数会覆盖 PowerShell 默认定义的函数，因此您可能想要以其定义为起点：

```powershell
Get-Content function:\TabExpansion2
```

有关自定义制表符扩展的详细信息，请参阅 [配方 1.18](#ch01.xhtml#customize_tab_expansion)。

## 用户输入

