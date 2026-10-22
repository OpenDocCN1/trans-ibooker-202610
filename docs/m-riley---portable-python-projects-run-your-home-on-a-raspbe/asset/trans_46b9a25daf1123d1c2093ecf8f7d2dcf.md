

your phone screen.

在继续进行项目的下一个也是最后一步之前，请验证一切是否按预期工作。

## Movement Notification

我们几乎完成了。现在只需要将第 6 章 *Hue Auto Light* 中的 motionsensortest.py 代码与 phototest.py 脚本合并到一个名为 photohook.py 的新 Python 文件中。以下是该文件最终内容的示例：

```
photohook/photohook.py

import os
import requests
import smbus
import time

WEBHOOK_URL = 'WEBHOOK_URL_GOES_HERE'
CAPTURE_FILE = r'/home/pi/projects/photohook/capture.jpg'

DEVICE_BUS = 1
DEVICE_ADDR = 0x17

TEMP_REG = 0x01
LIGHT_REG_L = 0x02
LIGHT_REG_H = 0x03
STATUS_REG = 0x04
ON_BOARD_TEMP_REG = 0x05
ON_BOARD_HUMIDITY_REG = 0x06
ON_BOARD_SENSOR_ERROR = 0x07
BMP280_TEMP_REG = 0x08
BMP280_PRESSURE_REG_L = 0x09
BMP280_PRESSURE_REG_M = 0x0A
BMP280_PRESSURE_REG_H = 0x0B
BMP280_STATUS = 0x0C
MOTION_DETECT = 0x0D

bus = smbus.SMBus(DEVICE_BUS)

alert_trigger = False

while True:
```

```
aReceiveBuf = []
aReceiveBuf.append(0x00)
for i in range(TEMP_REG,MOTION_DETECT + 1):
    aReceiveBuf.append(bus.read_byte_data(DEVICE_ADDR, i))

if aReceiveBuf[MOTION_DETECT] == 1 :
    if alert_trigger != True:
        os.system('raspistill --width 640 --height 480 \
        --quality 75 -o ' + CAPTURE_FILE)
        photo = {'media': open(CAPTURE_FILE, 'rb')}
        requests.post(WEBHOOK_URL, files=photo)
        time.sleep(5) # Wait 5 seconds before capturing another photo
        alert_trigger = True
    else:
        if alert_trigger != False:
            alert_trigger = False

time.sleep(1)
```

使用常规语法保存并运行脚本：

```
$ python3 photohook.py
```

脚本运行后，走到 Pi 前面，触发红外传感器。你应该会看到红外 LED 指示灯在检测到移动时亮起。几秒钟后，检查你的 Discord 客户端，确认 Pi 相机确实捕捉到了事件，并且图像已通过 PhotoBot 分配的 webhook URL 发布到你的 #photos 频道。

测试相机的位置和角度，并将 Sensorhub 的方向定位，使其能够在你想要的目标角度和位置最佳地捕捉移动。

如果希望 PhotoHook 在 Pi 上后台自动运行，请设置一个 Supervisor 脚本。参考以下 .conf 文件：

```
photohook/photohook.conf
```

```
[program:photohook]
```

```
directory = /home/YOURUSERNAME/projects/photohook
command = /usr/bin/nohup /usr/bin/python3 photohook.py &
user = YOURUSERNAME
environment=HOME="/home/YOURUSERNAME",USER="YOURUSERNAME"
autostart = true
autorestart = true
stdout_logfile = /var/log/supervisor/photohook.log
stderr_logfile = /var/log/supervisor/photohook_err.log
```

重启 Supervisor 服务或重启你的 Pi，并验证你的 PhotoBot 是否正在向你的 Discord #photos 频道传输捕捉到的运动照片。

## Next Steps

恭喜！你刚刚以与同类商用安防摄像头功能相当但成本仅为其一小部分的价格，打造了一台带通知更新的运动检测安防摄像头。与那些第三方黑盒替代品不同，你也完全清楚所捕捉的图像存储、传输和显示的位置和方式。最棒的是，你不必为使用这个自己打造的定制解决方案支付任何月费或订阅费。

如果你更喜欢事件通知传递的简便性和弹出通知方式，可以考虑重温过去的项目。将电子邮件通知机制替换为 Discord webhook。或者将两者结合使用，以实现重要信号的冗余故障转移消息传递。结合 IR 控制脉冲与快照，以提供 IR 控制设备按指令打开或关闭的照片证明。

监控 Pi 上的磁盘空间，并在存储容量降至 10% 以下时发送短信。将 PhotoHook 代码与 Hue AutoLight 项目合并，以在昏暗的房间中打开灯光，暂停几秒钟，拍摄照片并发布到你的 Discord 频道。这些项目展示了你的 Pi 仅用少量代码能做到多少。

### Footnotes

[90] https://discord.com/developers/docs/resources/webhook

[91] https://www.raspberrypi.org/documentation/raspbian/applications/camera.md

[92] https://discord.com/

[93] https://motion-project.github.io/index.html

## Chapter 12

## Continuous Improvement

你做到了。你。做。到了！

恭喜你完成了旅程并完成了本书中的项目。基于你获得的知识和经验进行构建，创建自己的项目。你拥有实现各种应用和自主硬件及软件解决方案所需的技能、承诺和毅力。

将各种自动化想法链接起来形成一个连贯的工作流程。例如，当我让我的 Pi “打开电脑” 时，它会结合 IR 发射器打开我的外部扬声器和 Hue 控制打开桌灯……当然也会打开我的电脑。

一旦你对书中使用的 Python 脚本更熟悉了，就尝试通过更改某些值、修改例程并使用其他 Pip 安装的 Python 库对其进行扩展来进行实验。你的唯一限制就是你的想象力。

在踏上自己的道路时，请牢记以下建议。

### Maintain

任何维护良好的软件项目都是不断演进的。保持你使用的最新 Python 语言版本和库的更新。对你在这些和其他 Python 项目中安装的第三方库运行 `pip3 install --upgrade`。此类升级会添加功能、修复错误、修补安全漏洞，并保持相关库的活跃和相关性。

也维护你自己的项目。随着你学习新的、改进的软件设计技术，将其应用到旧代码中，使其达到现代标准。随着新 Pi 型号的发布而升级。未来 Pi 产品路线图中的迭代肯定会提供更快的 CPU、GPU、网络，以及更多 RAM 和 I/O 选项。它们还将提供同时运行更大程序和执行更复杂操作的更大容量。

### Explore

研究其他 Pi 兼容的硬件配件，它们可以帮助你的想法变为现实。访问 Raspberry Pi 网站，获取销售附加组件、显示器、传感器、执行器和其他可将 Pi 提升到其设计者无法想象高度的电子产品的供应商列表。像 Adafruit Industries、[94] Chicago Electronic Distributors、[95] 和 Sparkfun Electronics [96] 这样的公司拥有充满 Pi 相关产品的在线目录，其中许多产品页面上都突出显示了入门项目文章。

了解 Python 的高级功能，例如异步和协程编程。搜索网络以查找提供巧妙函数或更智能执行流行例程（如文件操作和数据分析）方式的代码片段。将这些片段汇编到笔记本中。我保留了一套分类的 Jupyter [97] 笔记本，其中包含参考资料和剪辑的 Python 代码示例，以便快速检索和查看。Jupyter 笔记本最酷的功能是能够在笔记本本身内执行 Python 脚本。想象一本传统的数学教科书，再加上根据不同变量实时执行方程式并查看结果的能力，这就是使用 Jupyter 笔记本的感觉。微软的 Visual Studio Code 编辑器具有出色的 [98] Jupyter 笔记本支持，我最常在那里调用我的笔记本集合。至于在自己的笔记本集合中查找要存储的代码片段宝石，开发者中心的网站如 DevTalk [99] 和 StackOverflow [100] 在搜索特定编码问题的答案时可能很有用。

### Enjoy

花时间享受你劳动的成果。欣赏你的知识和时间投入如何让你提前进入未来，早在这些极客项目成为大众商业产品之前。向他人展示你制作的东西，激励他们也打造自己的项目，就像我希望这本书能激励你做同样的事情一样。

最重要的是，要玩得开心！

### Footnotes

+   [94] https://www.adafruit.com/
+   [95] https://chicagodist.com/
+   [96] https://www.sparkfun.com/
+   [97] https://jupyter.org/
+   [98] https://code.visualstudio.com/docs/python/jupyter-support
+   [99] https://www.devtalk.com/
+   [100] https://stackoverflow.com/

