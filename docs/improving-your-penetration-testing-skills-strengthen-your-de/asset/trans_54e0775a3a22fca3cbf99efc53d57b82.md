
- `print_good("message")`: 此调用用于提供任何操作的执行结果。消息会在开头加上 `[+]` 前缀，表示操作成功：

    >> print_good("HackingAlert")
    [+] HackingAlert
    => nil

- `print_error("message")`: 此调用用于显示脚本执行期间可能出现的错误消息。消息会在开头加上 `[-]` 前缀：

    >> print_error("HackingAlert")
    [-] HackingAlert
    => nil

我之所以讨论这些不同的打印调用，是因为在编写 Meterpreter 脚本时它们在各自情况下被广泛使用。你可以在 `/usr/share/metasploit-framework/documentation` 中找到与 Meterpreter API 相关的文档。阅读它们以获得清晰详细的理解。你也可以参考 `/usr/share/metasploit-framework/lib/rex/post/meterpreter`，其中包含许多与 Meterpreter API 相关的脚本。

这些脚本中包含各种 Meterpreter 核心、桌面交互、特权操作以及更多命令。审阅这些脚本，以便深入了解 Meterpreter 在被入侵系统中的运行方式。

## Railgun——将 Ruby 变成武器

在上一节食谱中，我们看到了使用 Meterpreter API 运行 Ruby 脚本。让我们更进一步。假设我们想在受害者机器上进行远程 API 调用；最简单的方法是什么？Railgun 是显而易见的答案。它是 Meterpreter 的一个扩展，允许攻击者直接调用 DLL 函数。通常它用于调用 Windows API，但我们可以调用受害者机器上的任何 DLL。

### 准备工作

要开始使用 Railgun，我们需要在目标机器上有一个活动的 Meterpreter 会话。要启动 Ruby 解释器，我们将使用 `irb` 命令，如上一节食谱中所讨论的：

```
meterpreter > irb
[*] Starting IRB shell
[*] The "client" variable holds the meterpreter client
>>
```

### 如何操作...

在开始调用 DLL 之前，让我们先看看为了充分利用 Railgun 需要遵循的基本步骤：

- 1. 确定你想要调用的函数。
- 2. 在 https://msdn.microsoft.com/en-us/library/aa383749(v=vs.85).aspx 上查找该函数。
- 3. 检查函数所在的库（DLL）（例如 kernel32.dll）。所选的库函数可以调用为 client.railgun.dll_name.function_name(arg1, arg2, ...)。
- 4. Windows MSDN 库可用于识别在目标机器上调用的有用 DLL 和函数。
- 5. 让我们使用 client.sys.config.sysinfo API 调用来收集目标信息：
- 6. 如果我们只想要操作系统版本，可以使用 client.sys.config.sysinfo['OS']：

```
>> client.sys.config.sysinfo
=> {"Computer"=>"VAGRANT-2008R2",
"OS"=>"Windows 2008 R2 (Build 7601, Service Pack 1).",
"Architecture"=>"x64", "System Language"=>"en_US",
"Domain"=>"WORKGROUP", "Logged On Users"=>2}
```

```
>> client.sys.config.sysinfo['OS']
=> "Windows 2008 R2 (Build 7601, Service Pack 1)."
```

使用 Railgun 可能会是一次非常强大且令人兴奋的体验。你可以练习自己的调用和脚本来分析输出。然而，如果你想要调用的 DLL 或函数不是 Railgun 定义的一部分怎么办？在这种情况下，Railgun 也为你提供了添加自己的函数和 DLL 到 Railgun 的灵活性。我们将在下一节食谱中处理这个问题。

### 原理...

Railgun 是 Meterpreter 的一个扩展，允许我们在无需编译自己的 DLL 的情况下调用 Windows API。Railgun 可用于向被入侵目标进行远程 DLL 调用。远程 DLL 调用是渗透测试中的重要过程，因为它使我们能够完全控制被入侵目标以执行任何系统指令。

### 还有更多...

Railgun 目前支持 10 个不同的 Windows API DLL。你可以在以下文件夹中找到它们的定义：`/usr/share/metasploit-framework/lib/rex/post/meterpreter/extensions/stdapi/railgun/def.`

## 向 Railgun 添加 DLL 和函数定义

在上一节食谱中，我们专注于通过 Railgun 调用 Windows API DLL。在本节食谱中，我们将专注于向 Railgun 添加自己的 DLL 和函数定义。为此，我们应该了解 Windows DLL。可以在 http://www.rubydoc.info/search/github/rapid7/metasploit-framework?q=Railgun 找到的 Railgun 文档可以帮助你快速了解在添加函数定义时可使用的不同 Windows 常量。

### 如何操作...

向 Railgun 添加新的 DLL 定义是一项简单的任务。假设你想添加一个随 Windows 提供但在你的 Railgun 中不存在的 DLL；你可以在 /usr/share/metasploit-framework/lib/rex/post/meterpreter/extensions/stdapi/railgun/def 下创建 DLL 定义，选择 Linux、macOS 或 Windows 操作系统文件夹，并将其命名为 def_dllname.rb。

以下模板应演示 DLL 如何定义：

```
# -*- coding: binary -*-
module Rex
module Post
module Meterpreter
module Extensions
module Stdapi
module Railgun
module Def

class Def_somedll

  def self.create_dll(dll_path = 'somedll')
    dll = DLL.new(dll_path, ApiConstants.manager)

    # 1st argument = Name of the function
    # 2nd argument = Return value's data type
    # 3rd argument = An array of parameters
    dll.add_function('SomeFunction', 'DWORD', [
      ["DWORD","hwnd","in"]
    ])

    return dll
  end
end
end; end; end; end; end; end
```

- 1. 对于本节食谱，首先我们需要创建原始 def_shell32.rb Railgun DLL 定义的备份，以便我们可以编写自己的定义：

```
root@kali:~# cd /usr/share/metasploit-framework/lib/rex/post/meterpreter/extensions/stdapi/railgun/def/windows
root@kali:/usr/share/metasploit-framework/lib/rex/post/meterpreter/extensions/stdapi/railgun/def/windows# mv def_shell32.rb def_shell32.rb.bak
```

- 2. 要编写 DLL 定义，我们将首先指定使用的模块：

```
# -*- coding: binary -*-
module Rex
module Post
module Meterpreter
module Extensions
module Stdapi
module Railgun
module Def
```

- 3. 然后是类和 DLL 的位置：

```
class Def_windows_shell32

def self.create_library(constant_manager,
library_path = 'shell32')
dll = Library.new(library_path, constant_manager)
```

将此代码保存为 def_shell32.dll 将为 shell32.dll 创建一个 Railgun 定义。

- 4. 下一步是向 DLL 定义添加函数。如果你查看 Metasploit 中的 def_shell32.dll 脚本，你会看到 IsUserAnAdmin 函数已经添加到了其中：

```
dll.add_function('IsUserAnAdmin', 'BOOL', [])
```

该函数根据条件简单返回 true 或 false 的布尔值。同样，我们可以在 shell32.dll 中添加自己的函数定义。

### 原理...

要列出 shell32.dll DLL 定义的可用函数，请在 Meterpreter 会话中输入以下内容：

```
meterpreter > irb
[*] Starting IRB shell
[*] The "client" variable holds the meterpreter client

>> session.railgun.shell32.functions
```

```
=> {"IsUserAnAdmin"=>#
<Rex::Post::Meterpreter::Extensions::Stdapi::Railgun::
:LibraryFunction:0x00560acbbe91d8 @return_type="BOOL",
@params=[], @remote_name="IsUserAnAdmin",
@calling_conv="stdcall">}
>>
```

如你所见，现在我们可以使用 IsUserAnAdmin 函数了。

那么，让我们从 shell32.dll 调用 IsUserAnAdmin 函数并分析输出：

```
>> client.railgun.shell32.IsUserAnAdmin
=> {"GetLastError"=>0, "ErrorMessage"=>
"The operation completed successfully.", "return"=>true}
```

该函数返回 true，表明我们的会话以系统管理员身份运行。Railgun 为我们提供了灵活地执行那些未以模块形式存在的任务的能力。因此，我们不仅限于框架提供的那些脚本和模块；事实上，现在我们可以按需进行调用。

这对使用 Railgun 作为调用 Windows API 的强大工具的简短演示，具体取决于你的需求。你可以在 MSDN 库中查找各种有用的 Windows API 调用，并将其添加到 Railgun 中以增强框架的功能。它可用于调用驻留在目标机器上的任何 DLL。在下一节食谱中，我们将继续分析和编写自己的 Meterpreter 脚本。

## 远程注入 VNC 服务器

虚拟网络计算（VNC）是一种图形化桌面共享系统，使用远程帧缓冲（RFB）协议远程控制另一台计算机。

我们可以使用 Metasploit 的 VNC 注入有效载荷远程注入 VNC 服务器。在本节食谱中，我们将学习如何远程注入 VNC 服务器。

### 准备工作

必须在主机系统上安装 VNC 查看器才能看到目标系统抛出的 VNC 会话。在本节食谱中，我们将使用 VNC 查看器，它已预装在 Kali Linux 中。
