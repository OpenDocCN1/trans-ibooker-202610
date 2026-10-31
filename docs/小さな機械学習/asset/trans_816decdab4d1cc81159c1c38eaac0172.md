
```
test -f tensorflow/lite/micro/tools/make/gen/ \
  sparkfun_edge_cortex-m4/bin/hello_world.bin \
  &&  echo "Binary was successfully created" || echo "Binary is missing"
```

执行此命令后，控制台会显示 `Binary was successfully created`。

如果显示 `Binary is missing`，则表示构建过程中存在问题。如果是这样，可能在 `make` 命令的输出中找到问题的线索。

### 对二进制文件进行签名

将二进制文件部署到设备上需要使用加密密钥进行签名。那么让我们执行一些命令，以便对二进制文件进行签名，使其可以刷新到 SparkFun Edge。这里使用的脚本来自在执行 Makefile 时下载的 Ambiq SDK。

输入以下命令，设置可用于开发的虚拟加密密钥：

```
cp tensorflow/lite/micro/tools/make/downloads/AmbiqSuite-Rel2.0.0/ \
  tools/apollo3_scripts/keys_info0.py \
  tensorflow/lite/micro/tools/make/downloads/AmbiqSuite-Rel2.0.0/ \
  tools/apollo3_scripts/keys_info.py
```

接下来，执行以下命令创建签名后的二进制文件。必要时，将 `python3` 替换为 `python`：

```
python3 tensorflow/lite/micro/tools/make/downloads/ \
  AmbiqSuite-Rel2.0.0/tools/apollo3_scripts/create_cust_image_blob.py \
  --bin tensorflow/lite/micro/tools/make/gen/ \
  sparkfun_edge_cortex-m4/bin/hello_world.bin \
  --load-address 0xC000 \
  --magic-num 0xCB -o main_nonsecure_ota \
  --version 0x0
```

这样就创建了 `main_nonsecure_ota.bin` 文件。执行此命令是为了创建最终版本的可用文件，以便在下一步中使用的脚本刷新设备：

```
python3 tensorflow/lite/micro/tools/make/downloads/ \
  AmbiqSuite-Rel2.0.0/tools/apollo3_scripts/create_cust_wireupdate_blob.py \
  --load-address 0x20000 \
  --bin main_nonsecure_ota.bin \
  -i 6 \
  -o main_nonsecure_wire \
  --options 0x1
```

这样，在执行命令的目录中应该会生成一个名为 `main_nonsecure_wire.bin` 的文件。这就是要刷新到设备的文件。

### 刷新二进制文件

SparkFun Edge 将当前运行的程序保存在 1 兆字节的闪存中。如果想让板载运行新程序，则需要将该程序发送到板上，板会将其保存在闪存中并覆盖先前保存的程序。

该过程称为刷新。让我们说明其步骤。

#### 将编程器安装到基板上

要下载新程序到板上，请使用 SparkFun USB-C Serial Basic 串行编程器。该设备使计算机能够通过 USB 与微控制器通信。

要将该设备安装到板上，请执行以下步骤：

1. SparkFun Edge 侧面有一个 6 针排针。
2. 将 SparkFun USB-C Serial Basic 插入这些引脚，并确保每个设备上标有 BLK 和 GRN 的引脚正确对齐。

[图6-13中](#ch06.html#sparkfun_edge_serial_basic)显示了正确的放置方式。

![](assets/timl_0613.png)

#### 将编程器连接到计算机

接下来，通过 USB 将板连接到计算机。要对板进行编程，需要确定计算机为该设备分配的名称。最好的方法是列出连接前后计算机上的所有设备，以确认哪个设备是新的。

###### 警告

有人报告了编程器操作系统默认驱动程序相关的问题，因此强烈建议在继续之前安装[驱动程序](https://oreil.ly/Wkxaf)。

在通过 USB 连接设备之前，执行以下命令：

```
# macOS:
ls /dev/cu*

# Linux:
ls /dev/tty*
```

应该会输出类似以下的列表：

```
/dev/cu.Bluetooth-Incoming-Port
/dev/cu.MALS
/dev/cu.SOC
```

将编程器连接到计算机的 USB 端口，然后再次执行命令：

```
# macOS:
ls /dev/cu*

# Linux:
ls /dev/tty*
```

应该会输出额外的项目，如下例所示。新的项目名称可能不同。此新项目即为设备的名称：

```
/dev/cu.Bluetooth-Incoming-Port
/dev/cu.MALS
/dev/cu.SOC
/dev/cu.wchusbserial-1450
```

该名称用于引用设备。然而，根据编程器连接到的 USB 端口，该名称可能会发生变化，因此如果从计算机上拔下板并重新连接，可能需要重新检查名称。

###### 提示

有用户报告列表中会出现两个设备。如果出现两个设备，正确的一个以 "wch "开头。

确定设备名称后，将其保存在 shell 变量 `DEVICENAME` 中以便以后使用：

```
export DEVICENAME=<your device name here>
```

这是在以后执行需要设备名称的命令时使用的变量。

#### 运行脚本刷新板

要刷新板，需要将其置于接收新二进制文件的特殊“引导加载程序”状态。然后可以运行脚本将二进制文件发送到板上。

首先，创建一个指定波特率的环境变量。波特率是指向设备发送数据的速度：

```
export BAUD_RATE=921600
```

将以下命令粘贴到终端中。命令中的 `${DEVICENAME}` 和 `${BAUD_RATE}` 将被替换为上一节中设置的值。必要时，请勿忘记将 `python3` 替换为 `python`：

```
python3 tensorflow/lite/micro/tools/make/downloads/ \
  AmbiqSuite-Rel2.0.0/tools/apollo3_scripts/ \
  uart_wired_update.py -b ${BAUD_RATE} \
  ${DEVICENAME} -r 1 -f main_nonsecure_wire.bin -i 6
```

接下来，将板重置为引导加载程序状态并刷新板。在板上，找到标有 `RST` 和 `14` 的按钮，如[图6-](#ch06.html#sparkfun_edge_buttons)14所示。

![](assets/timl_0614.png)

执行以下步骤：

1. 确认板已连接到编程器，并且整体通过 USB 连接到计算机。
2. 在板上，按住标有 `14` 的按钮。继续按住。
3. 在按住标有 `14` 的按钮的同时，按下标有 `RST` 的按钮以重置板。
4. 在计算机上按 Enter 键运行脚本。继续按住 `14` 按钮。

此时屏幕上应该显示如下内容：

```
Connecting with Corvette over serial port /dev/cu.usbserial-1440...
Sending Hello.
Received response for Hello
Received Status
length =  0x58
version =  0x3
Max Storage =  0x4ffa0
Status =  0x2
State =  0x7
AMInfo =
0x1
0xff2da3ff
0x55fff
0x1
0x49f40003
0xffffffff
[...lots more 0xffffffff...]
Sending OTA Descriptor =  0xfe000
Sending Update Command.
number of updates needed =  1
Sending block of size  0x158b0  from  0x0  to  0x158b0
Sending Data Packet of length  8180
Sending Data Packet of length  8180
[...lots more Sending Data Packet of length  8180...]
```

继续按住按钮 `14`，直到显示 `Sending Data Packet of length 8180`。显示后可以松开按钮（继续按住也没问题）。

编程将在终端中继续输出行。最终会显示如下内容：

```
[...lots more Sending Data Packet of length  8180...]
Sending Data Packet of length  8180
Sending Data Packet of length  6440
Sending Reset Command.
Done.
```

这表明刷新成功。

###### 提示

如果编程输出以错误结束，请检查是否打印了 `Sending Reset Command.`。如果是，则即使出现错误，刷新也可能已成功。否则，可能刷新失败。请再次执行此步骤（可以省略环境变量的设置）。

## 测试编程

现在二进制文件应该已部署到设备上。按下标有 `RST` 的按钮重新启动板。应该可以看到设备的四个 LED 按顺序闪烁。干得好！

## 查看调试数据

调试信息在程序运行时由板记录。要查看它，只需使用波特率 115200 监控板的串行端口输出。以下命令在 macOS 和 Linux 上应该可以工作：

```
screen ${DEVICENAME} 115200
```

你会看到大量输出在飞快滚动！要停止滚动，请先按 Ctrl-A，然后立即按 Esc。之后可以使用箭头键探索输出。输出包含对 `x` 的各种值执行的推理结果：

```
x_value: 1.1843798*2^2, y_value: -1.9542645*2^-1
```

要使用 `screen` 停止显示调试输出，请按 Ctrl-A，然后立即按 K 键，再按 Y 键。

###### 注

`screen` 程序是一个方便的实用程序，用于连接到其他计算机。这次我们使用它来监听 SparkFun Edge 板通过串行端口记录的数据。如果使用 Windows，也可以使用此程序执行相同的操作。也可以使用 [`CoolTerm`](https://oreil.ly/sPWQP) 实现相同的功能。

## 自行更改

现在基本应用程序已部署，让我们尝试玩弄或更改它。应用程序代码可以在 `tensorflow/lite/micro/examples/hello_world` 文件夹中找到。编辑并保存，然后重复之前的步骤，将更改后的代码部署到设备上。

有一些事情可以尝试：

* 通过调整每个周期的推理次数来减慢或加快 LED 闪烁。
* 修改 `output_handler.cc` 以将基于文本的动画记录到串行端口。
* 使用正弦波控制其他组件，如附加 LED 或声音生成器。

# STMicroelectronics STM32F746G Discovery Kit

[STM32F746G是](https://oreil.ly/cvm4J) 是一款配备相对强大的 Arm Cortex-M7 处理器核心的微控制器开发板。

该板运行 Arm 的 [Mbed OS](https://os.mbed.com)，这是一种为简化嵌入式应用程序的构建和部署而设计的嵌入式操作系统。也就是说，本节中的许多指令可用于为其他 Mbed 设备构建。

STM32F746G 配有 LCD 屏幕，可以创建更精致的视觉显示。

## 处理 STM32F746G 的输出

现在可以使用整个 LCD 来玩，所以可以绘制漂亮的动画。让我们将屏幕的 `x 轴`设为推理次数，`y 轴`设为预测的当前值。

在该值应出现的位置绘制一个点，并使其在屏幕上移动，以便在 0 到 2π 的输入范围内循环。[图6-15](#ch06.html#stm_hello_world_animation) 是其线框图。

由于每次推理需要一定的时间，因此可以通过微调 `constants.cc` 中定义的 `kInferencesPerCycle` 来调整点的移动速度和流畅度。

[图6-16](#ch06.html#STM32F746_hello_world) 是编程运行时 [动画*.gif 的*](https://oreil.ly/1EM7C) 静态图像。

![](assets/timl_0615.png)

[图6-16](#ch06.html#STM32F746_hello_world) 是编程运行时 [动画*.gif 的*](https://oreil.ly/1EM7C) 静态图像。

![](assets/timl_0616.png)

实现 STM32F746G 输出处理的代码位于 [`hello_world/disco_f746ng/output_handler.cc`](https://oreil.ly/bj4iL)，它代替原始文件 `hello_world/output_handler.cc` 使用。

让我们看看它：

```
#include "tensorflow/lite/micro/examples/hello_world/output_handler.h"
#include "LCD_DISCO_F746NG.h"
#include "tensorflow/lite/micro/examples/hello_world/constants.h"
```

首先是头文件。`output_handler.h` 指定了此文件的接口。`LCD_DISCO_F746NG.h` 由板制造商提供，声明了用于控制 LCD 屏幕的接口。此外，还包含了 `constants.h`，因为需要访问 `kInferencesPerCycle` 和 `kXrange`。

接下来，设置大量变量。`LCD_DISCO_F746NG` 在 `LCD_DISCO_F746NG.h` 中定义，提供控制 LCD 的方法：

```
// The LCD driver
LCD_DISCO_F746NG lcd;
```

`LCD_DISCO_F746NG` 类的详细信息发布在 [Mbed 的](https://oreil.ly/yiPHS) 网站上。

接下来，定义控制视觉外观和感觉的常量：
