
```
int main(int argc, char* argv[]) {
  setup();
  while (true) {
    loop();
  }
}
```

# 运行应用程序

这个示例包含与 macOS 兼容的音频提供程序。如果您能访问 Mac，则可以在开发机器上运行示例。首先，使用以下命令进行构建：

```
make -f tensorflow/lite/micro/tools/make/Makefile micro_speech
```

构建完成后，可以使用以下命令运行示例：

```
tensorflow/lite/micro/tools/make/gen/osx_x86_64/bin/micro_speech
```

可能会出现请求麦克风访问权限的弹出窗口。如果允许，程序将开始。

请说“是”和“否”。应该会看到如下输出：

```
Heard yes (201) @4056ms
Heard no (205) @6448ms
Heard unknown (201) @13696ms
Heard yes (205) @15000ms
Heard yes (205) @16856ms
Heard unknown (204) @18704ms
Heard no (206) @21000ms
```

检测到的每个单词后面显示的数字是分数。默认情况下，命令识别组件仅在分数高于 200 时才认为匹配有效，因此显示的分数都将高于 200。

分数后面的数字是程序启动以来的毫秒数。

如果没有显示输出，请确认在 Mac 的声音菜单中已选择内置麦克风，并且其输入音量已足够提高。

我们已经知道该程序可以在 Mac 上运行。现在让我们在嵌入式硬件上运行它。

# 部署到微控制器

本节将代码部署到三种不同的设备：

* [Arduino Nano 33 BLE Sense](https://oreil.ly/ztU5E)
* [SparkFun Edge](https://oreil.ly/-hoL-)
* [ST 麦克电子 STM32F746G Discovery 套件](https://oreil.ly/cvm4J)

将分别介绍每个设备的构建和部署流程。

由于每个设备都有自己的音频捕获机制，因此 `audio_provider.cc` 的实现各不相同。输出也是如此，每个设备都有 `command_responder.cc` 的变体。

`audio_provider.cc` 的实现复杂且特定于设备，与机器学习没有直接关系。因此，本章不予说明。但是，[附录 B 中](#app02.html#appendix_arduino_audio) 有 Arduino 实现的逐步讲解。如果您需要在自己的项目中捕获音频，欢迎在自己的代码中重用这些实现。

在部署步骤的同时，我们还将介绍每个设备的 `command_responder.cc` 实现。首先是 Arduino。

## Arduino

在撰写本文时，内置麦克风的 Arduino 板只有 [Arduino Nano 33 BLE Sense](https://oreil.ly/hjOzL)，因此本节将使用它。如果使用其他 Arduino 板并自行安装麦克风，则需要实现自己的 `audio_provider.cc`。

Arduino Nano 33 BLE Sense 还内置了 LED，用于指示单词已被识别。

[图 7-7 是](#ch07.html#arduino_nano_sense_led_2) 强调了 LED 的电路板照片。

![](assets/timl_0602.png)

那么，让我们看看如何使用此 LED 来指示检测到单词。

### Arduino 对命令的响应

所有 Arduino 板都内置了 LED，并且有一个方便的常量 `LED_BUILTIN`，因此可以使用它来获取引脚号。引脚号因板而异。为了保持代码的可移植性，我们将输出使用的 LED 限制为一个。

我们要做的是这样：为了表明推理正在执行，每次推理时都让 LED 闪烁。但是，如果听到“是”这个词，则让 LED 亮起几秒钟。

“否”这个词呢？好吧，这只是一个演示，所以我们就不太在意了。但是，所有检测到的命令都会记录到串行端口，因此您可以连接设备并检查所有匹配项。

用于 Arduino 的替代命令响应器位于 [`arduino/command_responder.cc` 中](https://oreil.ly/URkYi)。让我们看看它的源代码。首先，包含命令响应器的头文件和 Arduino 平台库的头文件：

```
#include "tensorflow/lite/micro/examples/micro_speech/command_responder.h"
#include "Arduino.h"
```

接下来，开始函数的实现：

```
// Toggles the LED every inference, and keeps it on for 3 seconds if a "yes"
// was heard
void RespondToCommand(tflite::ErrorReporter* error_reporter,
                      int32_t current_time, const char* found_command,
                      uint8_t score, bool is_new_command) {
```

下一步是将内置 LED 的引脚设置为输出模式，以便可以打开和关闭它。这是在仅执行一次的 `if` 语句中完成的，这要归功于名为 `is_initialized` 的 `static bool`。`static` 变量在函数调用之间保持其状态：

```
static bool is_initialized = false;
if (!is_initialized) {
  pinMode(LED_BUILTIN, OUTPUT);
  is_initialized = true;
}
```

接下来，设置一些 `static` 变量，以便记录最后检测到“YES”的时间以及执行的推理次数：

```
static int32_t last_yes_time = 0;
static int count = 0;
```

有趣的部分从这里开始。如果参数 `is_new_command` 为 `true`，我们就知道听到了什么，因此使用 `ErrorReporter` 实例记录日志。但是，如果听到的内容是“是”，则通过检查 `found_command` 字符数组的第一个字符来判断，并保存当前时间并点亮 LED：

```
if (is_new_command) {
  error_reporter->Report("Heard %s (%d) @%dms", found_command, score,
                         current_time);
  // If we heard a "yes", switch on an LED and store the time.
  if (found_command[0] == 'y') {
    last_yes_time = current_time;
    digitalWrite(LED_BUILTIN, HIGH);
  }
}
```

接下来，实现在恰好 3 秒后关闭 LED 的行为：

```
// If last_yes_time is non-zero but was >3 seconds ago, zero it
// and switch off the LED.
if (last_yes_time != 0) {
  if (last_yes_time < (current_time - 3000)) {
    last_yes_time = 0;
    digitalWrite(LED_BUILTIN, LOW);
  }
  // If it is non-zero but <3 seconds ago, do nothing.
  return;
}
```

当 LED 关闭时，将 `last_yet_time` 设置为 `0`，并在下一次听到“yes”之前不进入此 `if` 语句。`return`。此导入语句很重要。它可确保如果听到“是”，后续的输出代码不会执行，LED 会保持亮起。

到目前为止的实现中，如果听到“yes”的声音，LED 会亮起约 3 秒钟。下一部分将切换 LED 在每次推理时的亮灭。但是，在“是”模式期间，如前所述的 `return`。

这是最后一块代码：

```
// Otherwise, toggle the LED every time an inference is performed.
++count;
if (count & 1) {
  digitalWrite(LED_BUILTIN, HIGH);
} else {
  digitalWrite(LED_BUILTIN, LOW);
}
```

通过在每次推理时递增 `count` 变量，我们记录执行的推理总数。在 `if` 条件内部，使用 `&` 运算符对 `count` 变量和数字 `1` 执行二进制 AND 运算。

通过对 `count` 和 `1` 执行 AND 操作，我们过滤掉 `count` 的所有位，除了最低位。如果最低位为 `0`，即 `count` 为奇数，则结果为 `0`。在 C++ 的 `if statement` 中，这被评估为 `false`。

否则，结果为 `1`，表示偶数。`1` 被评估为 `true`，因此 LED 在偶数时点亮，在奇数时熄灭。这就是切换的原因。

完成了！我们已经实现了 Arduino 的命令响应器。让我们实际运行它。

### 运行示例

部署此示例所需的物品如下：

* Arduino Nano 33 BLE Sense 板
* Micro USB 电缆
* Arduino IDE

#### 提示

由于自本书撰写以来构建步骤可能已更改，请检查 [`README.md` 中](https://oreil.ly/7VozJ) 最新步骤。

本书的项目可作为 TensorFlow Lite Arduino 库的示例代码使用。如果尚未安装库，请打开 Arduino IDE，从 Tools 菜单中选择 Manage Libraries。在出现的窗口中，搜索名为 `Arduino_TensorFlowLite` 的库并安装它。应该可以使用最新版本，但如果出现问题，本书中测试的版本为 1.14-ALPHA。

#### 注

该库可以从 TensorFlow Lite 团队 [下载](https://oreil.ly/blgB8) 或使用 TensorFlow Lite for Microcontrollers Makefile 自行生成。如果您希望使用后者，请参阅 [附录 A](#app01.html#appendix_arduino_library_zip)。

安装库后，如 [图7-](#ch07.html#arduino_examples_micro_speech)8 所示，`micro_speech` 示例将显示在 File 菜单的 Examples → `Arduino_TensorFlowLite` 中。

点击 `micro_speech` 加载示例。会出现一个新窗口，显示每个源文件的标签。第一个标签中的文件 `micro_speech` 等同于前面描述的 `main_functions.cc`。

![](assets/timl_0604.png)

#### 注

由于我们在 [“运行示例”中](#ch06.html#hello_world_running_the_example) 已经解释了 Arduino 示例的结构，这里不再重复说明。

要运行示例，请通过 USB 连接 Arduino 设备。确保在工具菜单的板下拉列表中选择了正确的设备类型，如 [图 7-9 中所示](#ch07.html#arduino_board_dropdown_2)。

![](assets/timl_0605.png)

如果设备名称未显示在列表中，则需要安装支持包。为此，请点击 Boards Manager。在出现的窗口中搜索设备并安装相应的支持包的最新版本。接下来，如 [图 7-10](#ch07.html#arduino_port_dropdown_2) 所示，请确保在 [Tools] 菜单的 [Port] 下拉列表中选择了设备的端口。

![](assets/timl_0606.png)

最后，在 Arduino 窗口中点击上传按钮（[图 7-11 中](#ch07.html#arduino_upload_button_2) 以白色突出显示），编译代码并上传到 Arduino 设备。

![](assets/timl_0607.png)

上传成功完成后，您应该会看到 Arduino 板的 LED 开始闪烁。

要测试程序，请说“是”。检测到“是”后，LED 将保持常亮约 3 秒钟。

#### 提示

如果程序无法识别您的“是”，请尝试连续说几次。

您也可以在 Arduino 的串行监视器中查看推理结果。为此，请从工具菜单打开串行监视器。在这里，说“yes”、“no”或其他单词。应该会看到类似 [图 7-12 的](#ch07.html#micro_speech_serial_monitor) 内容。

![](assets/timl_0712.png)

#### 注

您可能会注意到我们使用的模型很小也不完整，更擅长检测“是”而不是“否”。这是一个例子，说明优化小模型大小可能会导致准确性问题。本主题将在 [第 8 章](#ch08.html#chapter_training_micro_speech) 中讨论。

### 自行修改

部署应用程序后，让我们尝试修改代码！您可以在 Arduino IDE 中编辑源文件。保存时，系统会提示您将示例重新保存到新位置。完成后，点击 Arduino IDE 的上传按钮进行构建和部署。

以下是一些可以尝试的想法：

* 将示例更改为在说“否”而不是“是”时点亮 LED。
* 使应用程序对“是”和“否”的特定序列做出反应，就像秘密代码短语一样。
* 使用“yes”和“no”命令控制其他组件，如额外的 LED 或舵机。

## SparkFun Edge

SparkFun Edge 配备了麦克风和四种颜色的 LED：红、蓝、绿、黄。[图 7-13 中](#ch07.html#sparkfun_edge_leds_2) 显示了 LED 被强调的 SparkFun Edge。

![](assets/timl_0611.png)

### SparkFun Edge 对命令的响应

为了明确程序正在运行，让我们切换蓝色 LED 在每次推理时的亮灭。如果听到“是”，则点亮黄色 LED；如果听到“否”，则点亮红色 LED；如果听到未知命令，则点亮绿色 LED。

SparkFun Edge 的命令响应器实现在 [`sparkfun_edge/command_responder.cc` 中](https://oreil.ly/i-3eJ)。该文件以一些包含开始：

```
#include "tensorflow/lite/micro/examples/micro_speech/command_responder.h"
#include "am_bsp.h"
```
