### Chapter 9

#### Voice IR Control

像 Amazon Alexa 或 Google Assistant 这样的智能语音助手可与各种现代家庭自动化设备集成。网络启用的智能灯泡、插座、电视和娱乐系统通常允许这些智能助手通过语音集成并控制其功能。

然而，存在大量主要是传统和低端消费电子产品，它们不可联网。相反，它们提供通过红外信号无线与产品控件交互的能力。这种技术自 1970 年代以来就已在普通消费者手中，并至今仍在使用。红外遥控器是“哑”的，因为它们无法确认它们传输的信号是否成功实现了预期操作。红外发射器还要求与它们传输控制码的红外接收器之间有直接、无阻挡的视线。但红外仍然有其用途，并继续被设计到大量消费电子产品中。

幸运的是，一个 Pi 和一个即插即用的红外收发器可以为这些传统非网络消费电子产品带来网络控制。而且我们将在不需要凌乱的面包板、跳线或其他外露电子元件的情况下做到这一点。

##### Setup

以下是构建此项目所需的物品。

###### Hardware

- Irdroid transceiver[^69]
- 具有红外接收器和相关遥控器的消费电子电器或设备

###### Software

- Linux Infrared Remote Control (LIRC)[^70]
- pushbullet[^71] Python 库

###### Services

- IFTTT[^72]
- Pushbullet[^73]

本项目建立在第 8 章《Voice Wake on LAN》的工作之上。因此，如果你尚未在 IFTTT 和 Pushbullet 上创建账户，请重新查看上一个项目中如何操作的说明。在本项目中，我们将 Irdroid 红外收发器连接到 Pi 的 USB 端口，然后在之前完成的 voicewol.py 脚本中添加几行 Python 代码。这样做将使你能够仅用你的声音与所选的红外控制设备进行交互。

在将 Irdroid IR 收发器连接到 Pi 之前，为 Pi 安装 Linux Infrared Remote Control (LIRC) 服务和辅助应用程序：

```
$ sudo apt install lirc
```

接下来，将 Irdroid 收发器插入其中一个已通电的 Pi 可用 USB 端口，如下一张照片所示。你应该会看到 Irdroid 内部的蓝色 LED



![](img/1f3315bdb28676808673aba305da7446_197_0.png)

##### 连接 Irdroid 到高于 5.7 的 Linux 内核

本章中的 Irdroid 说明适用于 Linux 内核 5.7 或更低版本。如果您运行的是内核版本更高的 Raspberry OS 版本，只需将 Irdroid 插入 Pi 的 USB 端口，将设备设置为 /etc/lirc0，并在 lircd.conf 文件中将驱动程序分配保持为默认值。[74] 重启后，您的 Irdroid 应该可以响应并准备就绪！

验证 Irdroid 硬件是否确实被 Raspberry Pi OS 识别。在 Pi OS 的根级 /dev 目录中查找名为 ttyACM0 的设备。请注意，如果您恰好连接了另一个也使用 ACM 的 USB 设备，Pi 可能会将 Irdroid 硬件重新分配为 ttyACM1 或更高增量。如果您想绝对确定 Irdroid 已被识别，请在连接 Irdroid 之前移除所有其他 USB 设备。您可以通过在 Pi 的终端窗口中发出以下语句来确认 Irdroid 设备已连接：

```
$ ls /dev/ttyACM*
```

如果成功，终端将回显路径 /dev/ttyACM0。如果未找到，ls 命令结果会抱怨没有此类文件或目录。在继续之前，设备必须被识别非常重要。

现在编辑 LIRC 选项配置文件，告诉 LIRC 使用什么类型的驱动程序与 Irdroid 通信以及 Irdroid 的设备位置分配：

```
$ sudo nano /etc/lirc/lirc_options.conf
```

在文件中找到驱动程序和设备条目，并分别将其分配替换为 irtoy 和 /dev/ttyACM0：

```
[lircd] nodaemon = False
driver = irtoy
device = /dev/ttyACM0
```

保存对 lirc_options.conf 文件所做的分配更改并重启 Pi：

```
$ sudo reboot
```

Pi 重新上线后，打开一个终端窗口并测试 Irdroid 的接收器是否能识别传入的红外传输。使用您打算用 Irdroid 辅助的 Pi 远程控制的消费电子设备的可用红外遥控器，使用以下命令启动 mode2 应用：

```
$ mode2 --device /dev/ttyACM0
```

如果 Irdroid 已使用 irtoy 驱动程序正确注册，mode2 应用应响应以下输出：

```
Using driver irtoy on device /dev/ttyACM0
Trying device: /dev/ttyACM0
Using device: /dev/ttyACM0
```

现在将您选择的可用红外遥控器对准 Irdroid 并按下遥控器的任意按钮。您应该会看到终端窗口输出一系列数字，在 pulse XXXX 和 space XXXX 之间交替，其中 XXXX 代表一系列整数。如果您在屏幕上看不到任何结果，请确保您使用的红外遥控器与其关联的消费电子产品一起工作。还请确保将遥控器的红外发射器直接对准 Irdroid，因为红外需要视线才能工作。测试遥控器完成后，按 Ctrl+C 键退出 mode2 程序或关闭正在运行它的终端窗口。

我们几乎完成了 LIRC 的配置。您需要做的唯一主要配置是告诉它您使用的是哪种遥控器，以便 LIRC 知道在要求其执行功能时要传输的正确遥控器红外代码。如果您要控制的红外设备是流行的电视、DVD 播放器或立体声接收器，有人可能已经为您记录了红外控制代码。您可以浏览 LIRC 遥控器数据库[75] 查看您的设备是否已列出。如果有，请下载其相关的 .conf 文件并将其保存在 /etc/lirc/lirc.conf.d/ 目录中。您可以对所有希望从 Pi 程序化操作的红外遥控设备执行此操作。这是因为 LIRC 的遥控 conf 文件各自都有唯一名称来标识要传输的遥控代码。

如果您的特定红外遥控电视或其他设备未列在 LIRC 遥控器数据库中，您可以使用 Irdroid 内置的红外接收器来记录您所需遥控器发出的红外代码。例如，我有一台旧的 Logitech DTS 解码器，它为我的 PC 声卡发出的音频提供动力。由于 LIRC 遥控器数据库中不存在此设备的此类配置文件，我使用了 irrecord 来捕获并存储其遥控代码。手持遥控器，在终端窗口中运行 LIRC 的 irrecord 程序：

```
$ irrecord --disable-namespace
```

按照程序的屏幕说明操作，包括为遥控设备命名。例如，我将我的遥控记录设备称为 dts，这将我的捕获的红外代码保存到 Pi 主目录中的 dts.lircd.conf 文件中。自动生成的配置文件将根据您记录的内容而具有不同的名称。irrecord 程序会要求您以各种随机方式按下遥控器的所有按钮。当它捕获到足够的基线后，它将能够为被按下的相关遥控按钮分配特定的名称。相应地为每个按钮命名。例如，我将 DTS 遥控器上的电源按钮命名为 power，依此类推。一旦您使用 irrecord 捕获了遥控器的所有按钮，请按照程序的说明退出并保存配置文件。假设您从 Pi 的默认主目录执行了 irrecord 程序，您应该会在该目录中看到新生成的 NAME_OF_REMOTE.lircd.conf 文件，其中 NAME_OF_REMOTE 是您在录制过程中分配的名称。将此生成的配置文件复制到 LIRC 的配置目录：

```
$ sudo cp ./NAME_OF_REMOTE.lircd.conf /etc/lirc/lircd.conf.d
```

为了让 LIRC 识别此新配置文件，您可以重新启动 LIRC 守护进程或重启 Pi。我更喜欢重启，因为它快速并且还验证所做的配置更改是永久性的。

##### 测试

真相的时刻到来了。是时候看看在配置 LIRC 上投入的时间是否物有所值了。确保您为其捕获了红外代码的消费电子设备的红外接收器与 Irdroid 直接视线范围内。然后打开一个终端窗口并，假设您在 irrecord 会话期间记录了电源按钮命令，发出 irsend 命令打开设备。就我的 Logitech DTS 接收器而言，我发出以下命令来打开或关闭它：

```
$ irsend SEND_ONCE dts power
```

如果一切正常，您应该会看到您的红外控制设备启动。使用 irrecord 命名的其他命令和您记录的命令，使用 irsend 程序。当您对从 Pi 远程控制红外设备变得多么容易感到满意时，将 irsend 命令合并到使用 os.system() 调用的 Python 脚本中以以编程方式触发这些代码。例如，这里有一行 Python 代码可打开我的旧 Philips HDTV，这要归功于 Armenio Castro Vale 贡献给 LIRC 遥控器数据库的 Philips_RC-7843 配置文件：

```
os.system("irsend SEND_ONCE PHILIPS_RC-7843 KEY_POWER")
```

您甚至可以编写一个简单的 Python 脚本来执行多个红外发送命令。只需确保在每次传输之间留出足够的时间，以等待代码传输并在接收设备上启用状态更改即可。

##### 添加

太好了，我们现在可以以编程方式控制旧的红外接收消费电子产品。是时候将那一行 irsend 代码合并到我们在 Voice Wake on LAN 项目中创建和配置的 voicewol.py Python 脚本中了。您可以将其与我们与唤醒 PC 相关联的现有 WAKE_PC 正文标签相关联，也可以创建一个新的 IFTTT/Pushbullet 序列来控制与 Wake PC 事件触发器分开的设备。例如，我创建了一个名为 PWR_TV 的单独正文消息来模拟按下我的 Philips 电视上的电源按钮。然后我创建了一个新的 IFTTT 小程序，按照 Voice Wake on LAN 项目中描述的相同步骤，调用此新小程序“Turn on/off TV”，并分配了一句新短语。我不能使用短语“Turn on the TV”，因为它对 Google 来说太通用而无法解析。相反，我更具体地使用了短语“Turn on the Philips”，这对 Google 来说更具体，可以正确通过 IFTTT 路由。对于 Pushbullet 消息，我添加了上述 PWR_TV 标签。以下是合并这些添加后的最终修改的 voicewol-ir.py 脚本：

```
voiceir/voicewol-ir.py
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
```





```
print("Turning on your computer...")
os.system('wakeonlan ' + MAC)
pushes = pbullet.delete_pushes()
os.system('irsend SEND_ONCE dts power')
elif latest['body'] == "PWR_TV":
    print("Pressing TV remote power button")
    os.system('irsend SEND_ONCE PHILIPS_RC-7843 KEY_POWER')
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
    pbapp.run_forever()
except KeyboardInterrupt:
    print("Keypress received, exiting script.")
    pbapp.close()
```

Create your own sequence of IR-controllable actions using a variety of sequences and voice commands. Do keep in mind that IFTTT limits users subscribed to its free account tier a maximum of three applets. If you need more, you can upgrade to their paid Pro subscription plan.

##### Blast It

Update the path and name of the modified Python script in the related `voicewol.conf` supervisor file from the prior Voice Wake on LAN project. Restart the Supervisor service or reboot the Pi. Ask Google or Alexa to “Turn on the PC” or whatever other command you associated with your IR-controlled device, and your device should respond accordingly. If you encounter any issues, troubleshoot by making sure you can communicate with your device directly from the `irsend` command in the Terminal.

Nice job! You’ve just brought your previously dumb, non-networked consumer electronic device of the past into the smart, networked world of the present. You can also put your old remote in a junk drawer, making sure to remove the batteries so they don’t leak in the remote after years of forgotten disuse.

##### Next Steps

You’ve seen how easy it is to build upon the foundation of past projects. Besides the time it took to configure the Irdroid with the Pi, adding your IR-controlled device to an existing voice project literally took minutes to enable. You also have the benefit of adding more legacy IR control systems to your Pi and triggering those by voice or other means. For example, if you have a ceiling fan controlled by an IR remote, you can incorporate its associated `irsend` command with the temperature-triggering code from Chapter 5, *Hue Fan*, to turn the ceiling fan on and off, depending on the warmth in the room. Some window blinds are controlled via infrared triggers, and these could be combined with the code from Chapter 6, *Hue Auto Light*, to raise and lower, based on the amount of outdoor light or time of day.

You can also add other infrared devices in the same line of sight as the device you’re managing. Imagine a symphony of actions instantiated by a single voice command. If you choose to do so, be aware that it’s best that the other devices are designed and manufactured by entirely different companies. It’s unlikely that those other devices will use the same IR codes to activate their device’s customized features.

The next project will return to giving the Pi something else to do besides interacting with hardware. We’re going to build an autonomous software bot to save time retrieving and acting on timely information that’s relevant to you.

##### Footnotes

[69] https://www.irdroid.com/irdroid-usb-ir-transceiver/
[70] https://lirc.org/
[71] https://github.com/rbrcsk/pushbullet.py
[72] https://ifttt.com/

+   [73] https://www.pushbullet.com/
+   [74] https://irdroid.eu/irdroid-usb-infrared-transceiver-linux-kernels-greater-5-8/
+   [75] http://lirc-remotes.sourceforge.net/remotes-table.html
