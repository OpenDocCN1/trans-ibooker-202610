

这是与 SensorHub 上各种传感器通信所需初始化和地址分配代码。对于本项目，我们将只关注外部和板载温度传感器。我们将在第 6 章 *Hue Auto Light* 中调用 SensorHub 的运动和光传感器。SensorHub 上还配有一个气压传感器，但它不会在本书的任何项目中使用。

③ 此 While 语句将始终为 True，因此脚本将一直运行，直到你通过 Ctrl+C 从脚本执行中退出。

④ 如前所述，SensorHub 有两个温度传感器：一个板载传感器直接焊接在板上，以及一个外部温度探头，能更准确地检测室温。在运行脚本之前，请确保外部传感器探头已连接到 SensorHub。条件语句 elif aReceiveBuf[STATUS_REG] & 0x02 将检查探头是否已连接，并在未连接时报告。

⑤ 板载报告的温度采用摄氏度。对于更喜欢华氏度的人，我们进行快速转换，以便我们可以同时以摄氏度和华氏度打印温度。

⑥ 尽管我们将使用外部温度探头读数进行本项目，但板载传感器读数仍会被报告，只是为了说明两个传感器之间的温度差异。板载温度传感器 literally 位于 Pi 发热最严重的芯片顶部。随着这些芯片升温，该传感器周围的温度也会升高。据我的经验，板载和外部温度传感器读数之间的差异可能超过 10 摄氏度（20 华氏度）。

准备好后，运行脚本：

```
$ python3 temperaturesensor.py
```

注意在你的终端窗口中，每秒打印的板载和外部传感器的温度读数。

多次运行脚本以验证温度是否被准确报告。握住外部探头将其加热，并观察该热量在脚本输出中显示的读数中反映出来。

## 过于温暖

现在我们能够检测环境室温，让我们使用我们在第 4 章 Water Leak Notifier 中采用的相同方法，设置上下温度边界以报告温度超过设定阈值条件时。 我们也只会轮询外部温度探头，因此请确保它已连接并正常工作。在此示例中，将温度阈值设置为 80 华氏度：

```
huefan/temperaturetest.py

import smbus
import time

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
    aReceiveBuf = []

    aReceiveBuf.append(0x00)
```

```
for i in range(TEMP_REG,MOTION_DETECT + 1):
    aReceiveBuf.append(bus.read_byte_data(DEVICE_ADDR, i))

if aReceiveBuf[STATUS_REG] & 0x01 :
    print('The external temperature sensor range was exceeded.')
elif aReceiveBuf[STATUS_REG] & 0x02 :
    print('The external temperature sensor is disconnected.')
else :
    external_celsius = aReceiveBuf[TEMP_REG]
    external_fahrenheit = (external_celsius * 9/5) + 32
    if external_fahrenheit > 79:
        if alert_trigger != True:
            print("Temperature is above 79 degrees.")
            alert_trigger = True
    else:
        if alert_trigger != False:
            print("Temperature is below 80 degrees.")
            alert_trigger = False

time.sleep(1)
```

运行脚本并注意在你的终端窗口中打印的温度读数：

```
$ python3 temperaturetest.py
```

对着外部温度探头吹气或使用低热设置的吹风机向其吹暖空气。一旦温度读数达到或超过 80 度，该状态将在终端中打印。移走热源并等待温度下降。当探头周围的空气低于 80 度时，消息 Temperature under 80 degrees. 将在运行脚本的终端中显示。

在你提高和降低外部探头周围的温度时观察输出屏幕，然后退出正在运行的 temperaturetest.py 脚本。

我们现在有能力控制电源开关并感测温度。是时候将所有这些独立功能组合成一个可执行脚本了。

## 全部整合

将我们用于切换 Hue 智能插头的代码与外部温度测试脚本结合起来。这样做将使我们能够在温度超过 79 度时打开风扇，并在温度低于 80 度时关闭它。

我们还需要将采样率降低到每两分钟（120 秒）一次，而不是每秒一次。这将为智能插头打开和关闭风扇提供充足的时间。否则，随着温度在单个阈值度数之间波动，我们将不断地打开和关闭风扇：

```
huefan/huefan.py

from phue import Bridge
import smbus
import time

huebridge = Bridge('YOUR_HUE_BRIDGE_IP_GOES_HERE')

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
HUMAN_DETECT = 0x0D

bus = smbus.SMBus(DEVICE_BUS)
```

```
alert_trigger = False

while True:
    aReceiveBuf = []
    aReceiveBuf.append(0x00)
    for i in range(TEMP_REG,HUMAN_DETECT + 1):
        aReceiveBuf.append(bus.read_byte_data(DEVICE_ADDR, i))
    if aReceiveBuf[STATUS_REG] & 0x01 :
        print('The external temperature sensor range was exceeded.')
    elif aReceiveBuf[STATUS_REG] & 0x02 :
        print('The external temperature sensor is disconnected.')
    else :
        external_celsius = aReceiveBuf[TEMP_REG]
        external_fahrenheit = (external_celsius * 9/5) + 32
        if external_fahrenheit > 79:
            if alert_trigger != True:
                huebridge.set_light('Fan','on', True)
                alert_trigger = True
        else:
            if alert_trigger != False:
                huebridge.set_light('Fan','on', False)
                alert_trigger = False
    time.sleep(120)
```

保存并执行脚本。

```
$ python3 huefan.py
```

根据你理想的房间制冷条件调整温度和睡眠时间（即采样率）值。如果发现温度在 79 和 80 度之间反复波动，请进一步更改采样率。这样你就不会每两分钟不断地打开和关闭智能插头。你还可以创建一个嵌套条件语句，根据上次温度读数延长智能插头打开或关闭的持续时间。你可以根据喜好继续调整，以获得对房间温度变化响应的触发器的精细控制。

如果你打算让脚本始终运行，请为项目创建并启用一个 Supervisor 配置文件，并将其保存在 /etc/supervisor/conf.d 目录中：

```
huefan/huefan.conf
```

```
[program:huefan]
directory = /home/YOURUSERNAME/projects/huefan
command = /usr/bin/nohup /usr/bin/python3 huefan.py &
user = YOURUSERNAME
environment=HOME="/home/YOURUSERNAME",USER="YOURUSERNAME"
autostart = true
autorestart = true
stdout_logfile = /var/log/supervisor/huefan.log
stderr_logfile = /var/log/supervisor/huefan.log
```

重启 Supervisor 守护进程以激活受监督的 huefan.py Python 脚本。

```
$ sudo service supervisor restart
```

干得漂亮！你已能够将你在第 4 章 Water Leak Notifier 中学到的知识转化为基本上创建自己的恒温器。你现在拥有足够的经验和编码技能来连接并从 Pi 的输入中读取。你甚至可以通过包含我们在水传感器项目中编写的电子邮件警报代码来进一步扩展此项目。这样，即使你不在同一房间，也会被通知风扇已打开或关闭。

