
// The colors we'll draw
const uint32_t background_color = 0xFFF4B400;  // Yellow
const uint32_t foreground_color = 0xFFDB4437;  // Red
// The size of the dot we'll draw
const int dot_radius = 10;
```

颜色以十六进制值提供，如 `0xFFF4B400`。`AARRGGBB` 中 `AA` 表示 alpha 值（或不透明度，`FF` 为完全不透明），`RR`、`GG`、`BB` 表示红色、绿色、蓝色的量。

### 小贴士

`0xFFF4B400`。完全不透明的颜色，红色较多，绿色也相当多，因此呈现出漂亮的橙黄色。

也可以通过 Google 搜索来查询值。

接下来，声明一些定义动画形状和大小的变量：

```
// Size of the drawable area
int width;
int height;
// Midpoint of the y axis
int midpoint;
// Pixels per unit of x_value
int x_increment;
```

在变量之后，定义 `HandleOutput()` 函数，指示首次执行时要做什么：

```
// Animates a dot across the screen to represent the current x and y values
void HandleOutput(tflite::ErrorReporter* error_reporter, float x_value,
                  float y_value) {
  // Track whether the function has run at least once
  static bool is_initialized = false;

  // Do this only once
  if (!is_initialized) {
    // Set the background and foreground colors
    lcd.Clear(background_color);
    lcd.SetTextColor(foreground_color);
    // Calculate the drawable area to avoid drawing off the edges
    width = lcd.GetXSize() - (dot_radius * 2);
    height = lcd.GetYSize() - (dot_radius * 2);
    // Calculate the y axis midpoint
    midpoint = height / 2;
    // Calculate fractional pixels per unit of x_value
    x_increment = static_cast<float>(width) / kXrange;
    is_initialized = true;
  }
```

里面写了很多内容！首先，使用 `lcd` 的方法设置背景色和前景色。名为 `lcd.SetTextColor()` 的奇怪方法不仅设置文本的颜色，还设置所有绘制物体的颜色：

```
// Set the background and foreground colors
lcd.Clear(background_color);
lcd.SetTextColor(foreground_color);
```

接下来，为了掌握绘制圆的位置，计算实际可绘制的范围。如果弄错了，可能会尝试超出屏幕边缘绘制，导致意外的结果：

```
width = lcd.GetXSize() - (dot_radius * 2);
height = lcd.GetYSize() - (dot_radius * 2);
```

随后，确定负 `y` 值在屏幕上绘制的中央位置。还计算屏幕宽度的多少像素相当于 `x` 值的 1 个单位。为了得到浮点数结果，使用 `static_cast`：

```
// Calculate the y axis midpoint
midpoint = height / 2;
// Calculate fractional pixels per unit of x_value
x_increment = static_cast<float>(width) / kXrange;
```

与之前相同，使用名为 `is_initialized` 的 `static` 变量，使此块代码只执行一次。

初始化完成后，开始输出。首先清除之前的绘制：

```
// Clear the previous drawing
lcd.Clear(background_color);
```

接下来，计算 `x_value`，在显示器的 x 轴上绘制点的位置：

```
// Calculate x position, ensuring the dot is not partially offscreen,
// which causes artifacts and crashes
int x_pos = dot_radius + static_cast<int>(x_value * x_increment);
```

接下来是 `y`。这有点复杂，因为想要在 `midpoint` 上方绘制正值，在下方绘制负值：

```
// Calculate y position, ensuring the dot is not partially offscreen
int y_pos;
if (y_value >= 0) {
  // Since the display's y runs from the top down, invert y_value
  y_pos = dot_radius + static_cast<int>(midpoint * (1.f - y_value));
} else {
  // For any negative y_value, start drawing from the midpoint
  y_pos =
      dot_radius + midpoint + static_cast<int>(midpoint * (0.f - y_value));
}
```

一旦确定点的位置，就只需绘制点：

```
// Draw the dot
lcd.FillCircle(x_pos, y_pos, dot_radius);
```

最后，使用 `ErrorReporter` 将 `x` 和 `y` 值记录到串行端口：

```
// Log the current X and Y values
error_reporter->Report("x_value: %f, y_value: %f\n", x_value, y_value);
```

### 注释

`ErrorReporter` 可以通过 STM32F746G 的串行接口输出数据。这是由 [`micro/disco_f746ng/debug_log.cc`的](https://oreil.ly/eL1ft) 自定义实现所实现的，该实现取代了 [`micro/debug_log.cc`中](https://oreil.ly/HpJ-t) 的原始实现。

## 运行示例

接下来构建项目！STM32F746G 搭载了 Arm 的 Mbed OS，因此使用 Mbed 工具链将应用程序部署到设备上。

### 小贴士

由于构建步骤可能与本书撰写时有所不同，请检查 [`README.md`](https://oreil.ly/WuhIz) 以获取最新步骤。

开始之前，需要以下内容：

* STM32F746G Discovery Kit 板
* Mini USB 电缆
* Arm Mbed CLI（遵循 [Mbed 设置指南](https://oreil.ly/TkRwd)）
* Python 3 和 `pip`

与 Arduino IDE 类似，Mbed 需要以某种方式结构化源文件。TensorFlow Lite for Microcontrollers Makefile 知道这种方法，并会生成适合 Mbed 的目录。

为此，执行以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  TARGET=mbed TAGS="CMSIS disco_f746ng" generate_hello_world_mbed_project
```

结果会创建新的目录：

```
tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4/prj/ \
  hello_world/mbed
```

该目录包含以正确方式结构化、使 Mbed 能够构建的示例的所有依赖项。

首先，移动到该目录，以便在那里执行命令：

```
cd tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4/prj/ \
  hello_world/mbed
```

接下来，使用 Mbed 下载依赖项并构建项目。

首先，使用以下命令指定当前目录是 Mbed 项目的根目录：

```
mbed config root .
```

接下来，指示 Mbed 下载依赖项并准备构建：

```
mbed deploy
```

默认情况下，Mbed 使用 C++98 构建项目。然而，TensorFlow Lite 需要 C++11。执行以下 Python 代码片段以更改 Mbed 配置文件，使其使用 C++11。只需在命令行中输入或粘贴即可：

```
python -c 'import fileinput, glob;
for filename in glob.glob("mbed-os/tools/profiles/*.json"):
  for line in fileinput.input(filename, inplace=True):
    print(line.replace("\"-std=gnu++98\"","\"-std=c++11\", \"-fpermissive\""))'
```

最后，执行以下命令进行编译：

```
mbed compile -m DISCO_F746NG -t GCC_ARM
```

结果应该在以下路径生成二进制文件：

```
cp ./BUILD/DISCO_F746NG/GCC_ARM/mbed.bin
```

使用像 STM32F746G 这样的 Mbed 兼容板的好处之一是部署非常简单。只需将 STM 板连接并复制文件即可。在 macOS 上，可以使用以下命令执行此操作：

```
cp ./BUILD/DISCO_F746NG/GCC_ARM/mbed.bin /Volumes/DIS_F746NG/
```

或者，在文件浏览器中查找 `DIS_F746NG` 卷并拖放文件。复制文件后，闪存过程将开始。完成后，设备的屏幕上应该显示动画。

除了此动画外，程序运行期间调试信息也会记录到板上。要查看它，请以 9600 波特率建立与板的串行连接。

在 macOS 和 Linux 上，执行以下命令应该会列出设备：

```
ls /dev/tty*
```

应该如下所示：

```
/dev/tty.usbmodem1454203
```

确定设备后，使用以下命令连接。将 `/dev/tty.devicename` 替换为 `/dev` 中显示的设备名称：

```
screen /<dev/tty.devicename> 9600
```

你会看到大量输出在飞过。要停止滚动，请按 Ctrl-A，然后立即按 Esc。之后，可以使用箭头键探索输出。输出包含针对 `x` 的各种值的推理结果：

```
x_value: 1.1843798*2^2, y_value: -1.9542645*2^-1
```

要停止在 `screen` 中显示调试输出，请按 Ctrl-A，然后立即按 K 键，然后按 Y 键。

## 自己修改

现在应用程序已部署，玩玩也很有趣！应用程序代码可以在 `tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4/prj/hello_world/mbed` 文件夹中找到。编辑并保存，然后重复之前的步骤将修改后的代码部署到设备上即可。

有一些事情可以尝试：

* 通过调整每个周期推理的次数来减慢或加快点的移动。
* 修改 `output_handler.cc`，将基于文本的动画记录到串行端口。
* 使用正弦波控制 LED 或声音生成器等其他组件。

# 总结

在前三章中，我们从头到尾完成了模型的训练、转换为 TensorFlow Lite、创建应用程序以及部署到小型设备。接下来的章节将探索更精致、更令人兴奋的示例，以将嵌入式机器学习付诸实践。

首先，创建一个使用 18KB 小型模型来识别语音命令的应用程序。

# 第7章. 唤醒词检测：构建应用程序

本作品使用 AI 翻译。欢迎提供意见和反馈：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

TinyML 可能是新现象，但其最广泛的应用可能已经在你的家中、汽车中或口袋里使用。你知道那是什么吗？

近年来，数字助理正在兴起。这些产品提供语音用户界面（UI），无需屏幕或键盘即可即时访问信息。在 Google Assistant、Apple 的 Siri 和 Amazon Alexa 之间，这些数字助理几乎无处不在。几乎所有手机都内置了某种类型，从旗舰机型到面向新兴市场的语音优先设备。此外，它们还被集成到智能扬声器、计算机和汽车中。

在大多数情况下，语音识别、自然语言处理以及生成对用户查询的响应等繁重工作都在云端进行，由运行大型 ML 模型的强大服务器执行。用户提问时，语音作为流发送到服务器。服务器理解其含义，查找所需信息并返回适当的响应。

然而，助理的魅力之一是始终在线，随时准备帮助你。说“嘿，Google”或“Alexa”，即可启动助理，无需按按钮即可传达所需内容。也就是说，无论你坐在客厅、驾驶高速公路，还是在大自然中拿着手机，都必须 24/7 全天候聆听你的声音。

在服务器上进行语音识别很容易，但持续将语音流从设备发送到数据中心是不现实的。从隐私角度来看，每秒将语音发送到远程服务器是绝对的灾难。即使勉强可行，也需要巨大的带宽，会在几小时内耗尽移动数据计划。此外，网络通信消耗能量，持续发送数据会迅速耗尽设备电池。此外，由于所有请求都要往返服务器，助理会感觉滞后，响应缓慢。

助理真正需要的语音只是唤醒词（例如“嘿 Google”）之后的语音。如果能在不发送数据的情况下检测到该词，并在听到时开始流式传输会怎样？这可以保护用户隐私，节省电池寿命和带宽，并在无需等待网络的情况下唤醒助理。

这就是 TinyML 大显身手的地方。可以训练一个监听唤醒词的小型模型，并在低功耗芯片上运行。将它集成到手机中，就能持续监听唤醒词。听到魔咒后，手机会通知操作系统（OS），OS 开始捕获语音并将其发送到服务器。
