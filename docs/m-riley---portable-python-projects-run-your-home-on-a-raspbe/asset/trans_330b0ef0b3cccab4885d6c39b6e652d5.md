

```
except:
    print("No messages.")

⑤ pb = Pushbullet(ACCESS_CODE)

pbapp = Listener(account=pb,
    on_push=on_push,
    http_proxy_host=HTTP_PROXY_HOST,
    http_proxy_port=HTTP_PROXY_PORT)
try:
⑥    pbapp.run_forever()
except KeyboardInterrupt:
    print("Keypress received, exiting script.")
    pbapp.close()
```

让我们简要回顾一下此脚本中部分代码的作用。

+   ① 从 pushbullet Python 库导入 Listener 和 Pushbullet 函数。
+   ② 将 PUSHBULLET_ACCESS_CODE_GOES_HERE 替换为你之前为登录生成的 Pushbullet 访问代码。将其值设为 ACCESS_CODE 静态变量。
+   ③ 使用此 on_push() 函数来捕获来自 Pushbullet 事件的推送。
+   ④ 由于你只关心查看收到的第一个 PushBullet 消息，仅使用 print 语句显示 Pushbullet 消息的主体。在最终脚本中，我们将删除队列中的所有消息，以免重新处理之前收到的 Pushbullet 消息。
+   ⑤ 在这里，我们启动 Pushbullet 服务并将其分配给 pb 对象。然后将该对象连同 ACCESS_CODE 以及如有必要的代理设置一起传递给 Listener() 函数。我们将其实例化结果分配给 pbapp 对象。
+   ⑥

最后，我们指示 pbapp 实例运行_forever()，或至少直到我们通过 Ctrl+Z 键盘组合退出程序为止。

保存并运行 pbreceiver.py Python 脚本：

```
$ python3 pbreceiver.py
```

脚本运行时，让你的智能助手说“Turn on the computer”，几秒钟后，你应该会在终端上看到消息主体打印出来：

```
Latest message body received:
```

```
WAKE_PC
```

如果没有看到任何消息出现，请登录 IFTTT 网站并仔细检查配置。查看 applet 的活动以确保它已运行并在 Pushbullet 便笺的正文中发送了 WAKE_PC 文本。继续之前请验证 pbreceiver.py 脚本是否正常工作。你可以通过退出正在运行的脚本或直接关闭其运行的终端窗口来退出程序。

## Wake Up

我们已准备好迎接关键时刻。将 wakeup.py 脚本与 pbreceiver.py 脚本结合，并在执行 wakeonlan 命令后添加一行代码来清除所有收到的 Pushbullet 消息。创建一个名为 voicewol.py 的新 Python 脚本，并在其中包含以下代码：

```
voicewol/voicewol.py

import os
from pushbullet import Listener
from pushbullet import Pushbullet

ACCESS_CODE = 'PUSHBULLET_ACCESS_CODE_GOES_HERE'
HTTP_PROXY_HOST = None
HTTP_PROXY_PORT = None
MAC = 'YOUR_COMPUTER_MAC_ADDRESS_GOES_HERE'

def on_push(data):
    pbullet = Pushbullet(API_KEY)
    pushes = pbullet.get_pushes()
    try:
        latest = pushes[0]
        if latest['body'] == "WAKE_PC":
            print("Turning on your computer...")
            os.system('wakeonlan ' + MAC)
            pushes = pbullet.delete_pushes()
        else:
            print("Unrecognized code in message body.")
    except:
        print("No messages.")

pb = Pushbullet(ACCESS_CODE)

pbapp = Listener(account=pb,
    on_push=on_push,
    http_proxy_host=HTTP_PROXY_HOST,
    http_proxy_port=HTTP_PROXY_PORT)
try:
```

```
pbapp.run_forever()
except KeyboardInterrupt:
    print("Keypress received, exiting script.")
    pbapp.close()
```

通过将脚本名称传递给 Python 3 解释器来保存并运行脚本。

```
$ python3 voicewol.py
```

将目标计算机置于睡眠状态，然后说出唤醒它的魔咒短语。

最后，为 Supervisor 创建一个 .conf 文件以供其消费并在 Pi 重启时启动脚本，并在因错误、低内存条件或其他意外异常而失败时重启它：

```
voicewol/voicewol.conf

[program:voicewol]
command = /usr/bin/nohup /usr/bin/python3 \
/home/pi/projects/voicewol/voicewol.py &
directory = /home/pi/projects/voicewol
user = pi
environment=HOME="/home/pi",USER="pi"
autostart = true
autorestart = true
stdout_logfile = /var/log/supervisor/voicewol.log
stderr_logfile = /var/log/supervisor/voicewol_err.log
```

将 voicewol.conf 文件保存在 /etc/supervisor/conf.d 目录中，然后重新启动 Supervisor 服务，或更好的是，重新启动你的 Pi。在等待 Pi 重启的同时，将目标桌面计算机置于睡眠状态：

```
$ sudo reboot
```

等待几分钟让 Pi 重新启动，然后对你的智能助手说“Turn on the computer”。你的睡眠中的桌面计算机应该会唤醒并准备好接收你的命令。

太棒了！你已经获得了仅通过要求智能语音助手即可唤醒计算机的便利。在多次尝试以验证其一致工作后，用你日益增长的技术知识和创造力让朋友和家人惊叹，展示你如何利用未来的人机交互。

## Next Steps

通过将 IFTTT 和 PushBullet 服务与来自 Amazon 或 Google 的智能助手以及一个简短的 Python 脚本结合，你现在可以让 Raspberry Pi 用你的声音打开计算机。在这个基础 IFTTT applet 工作后，你可以用你的声音激活家庭中的其他服务。控制执行器以解锁并打开门，升降电动升降办公桌，并打开洒水系统——所有这些都可以用你的声音实现。反向消息流以将 Pushbullet 消息传输到其他互联网连接设备。在你的智能手机上安装 Pushbullet 客户端，并用它代替我们在之前项目中使用的电子邮件警报。

你还可以增强 voicewol.py 脚本以响应“Turn off the computer”，让你的 Pi 传输适当的命令以使目标桌面进入睡眠状态。macOS 已默认安装 OpenSSH 服务器。Windows 10 的 OpenSSH 服务器可以通过在 Windows 10 应用和功能设置页面中选择可选功能轻松安装。

下一个项目将在构建的 IFTTT + Pushbullet 组合的基础上增加红外功能。这样就可以激活和控制依赖于传统视线、发射即忘通信协议的电器。

## Footnotes

[63] https://packages.debian.org/buster/wakeonlan
[64] https://github.com/rbrcsk/pushbullet.py
[65] https://ifttt.com/
[66] https://www.pushbullet.com/
[67]
https://pypi.org/project/pushbullet.py/#files
[68] https://ifttt.com/maker_webhooks/details

# Chapter 9

# Voice IR Control

像 Amazon Alexa 或 Google Assistant 这样的智能语音助手可与各种现代家庭自动化设备集成。网络启用的智能灯泡、插座、电视和娱乐系统通常允许这些智能助手通过语音集成并控制其功能。

然而，存在大量主要是传统和低端消费电子产品，它们不可联网。相反，它们提供通过红外信号无线与产品控件交互的能力。这种技术自 1970 年代以来就已在普通消费者手中，并至今仍在使用。红外遥控器是“哑”的，因为它们无法确认它们传输的信号是否成功实现了预期操作。红外发射器还要求与它们传输控制码的红外接收器之间有直接、无阻挡的视线。但红外仍然有其用途，并继续被设计到大量消费电子产品中。

幸运的是，一个 Pi 和一个即插即用的红外收发器可以为这些传统非网络消费电子产品带来网络控制。而且我们将在不需要凌乱的面包板、跳线或其他外露电子元件的情况下做到这一点。

## Setup

以下是构建此项目所需的物品。

## Hardware

- Irdroid transceiver[^69]
- 具有红外接收器和相关遥控器的消费电子电器或设备

## Software

- Linux Infrared Remote Control (LIRC)[^70]
- pushbullet[^71] Python 库

## Services

- IFTTT[^72]
- Pushbullet[^73]

本项目建立在第 8 章《Voice Wake on LAN》的工作之上。因此，如果你尚未在 IFTTT 和 Pushbullet 上创建账户，请重新查看上一个项目中如何操作的说明。在本项目中，我们将 Irdroid 红外收发器连接到 Pi 的 USB 端口，然后在之前完成的 voicewol.py 脚本中添加几行 Python 代码。这样做将使你能够仅用你的声音与所选的红外控制设备进行交互。

在将 Irdroid IR 收发器连接到 Pi 之前，为 Pi 安装 Linux Infrared Remote Control (LIRC) 服务和辅助应用程序：

```
$ sudo apt install lirc
```

接下来，将 Irdroid 收发器插入其中一个已通电的 Pi 可用 USB 端口，如下一张照片所示。你应该会看到 Irdroid 内部的蓝色 LED

