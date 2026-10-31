
# TensorFlow Lite Micro 调试日志移植指南

```
#include "Arduino.h"

// The Arduino DUE uses a different object for the default serial port shown in
// the monitor than most other models, so make sure we pick the right one. See
// https://github.com/arduino/Arduino/issues/3088#issuecomment-406655244
#if defined(__SAM3X8E__)
#define DEBUG_SERIAL_OBJECT (SerialUSB)
#else
#define DEBUG_SERIAL_OBJECT (Serial)
#endif

// On Arduino platforms, we set up a serial port and write to it for debug
// logging.
void DebugLog(const char* s) {
  static bool is_initialized = false;
  if (!is_initialized) {
    DEBUG_SERIAL_OBJECT.begin(9600);
    // Wait for serial port to connect. Only needed for some models apparently?
    while (!DEBUG_SERIAL_OBJECT) {
    }
    is_initialized = true;
  }
  DEBUG_SERIAL_OBJECT.println(s);
}

int main(int argc, char** argv) {
  DebugLog("Hello World!\n");
}
```

这些示例都创建了串行对象，并假设用户通过 USB 将微控制器进行串行连接并连接到主机。

移植工作中的重要第一步是为平台创建一个最小示例，该示例在您想要使用的 IDE 中运行，并以某种方式将字符串输出到主机控制台。如果成功，该代码将成为添加到 TensorFlow Lite 代码中的专用函数的基。

## 实现 DebugLog()

查看 [`TensorFlow/lite/micro/debug_log.cc`查看](https://oreil.ly/Lka3T) ，可以看到与最初介绍的 "hello world " 示例非常相似的 `DebugLog()` 函数实现，它使用 `stdio.h` 和 `fprintf()` 将字符串输出到控制台。如果您的平台完全支持标准 C 库，并且不担心额外的二进制大小，那么可以使用此默认实现并忽略本节的其余部分。但是，遗憾的是，更有可能需要使用另一种方法。

作为第一步，我们将使用已存在的 `DebugLog()` 函数的测试。首先，执行以下命令行：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  generate_micro_error_reporter_test_make_project
```

[`TensorFlow/lite/micro/tools/make/gen/linux_x86_64/prj/micro_error_reporter_test/make/`](...)（请尝试将 `Linux` 替换为 `osx` 或 `Windows`）中查看，`tensorflow` 或 `third_party` 等文件夹应该存在。这些文件夹包含 C++ 源代码，将所有文件拖到 IDE 或构建系统并编译后，应该会完成一个用于测试错误报告功能的执行文件。最初尝试构建此代码很可能会失败。因为此代码仍在使用 [`debug_log.cc`的](https://oreil.ly/fDkLh) 默认的 `DebugLog()` 实现，并且依赖于 `stdio.h` 和 C 标准库。为避免此问题，请更改 `debug_log.cc`，删除 `#include <cstdio>` 语句，并将 `DebugLog()` 的实现替换为不执行任何操作的内容：

```
#include "tensorflow/lite/micro/debug_log.h"

extern "C" void DebugLog(const char* s) {
  // Do nothing for now.
}
```

在此更改状态下，使源文件集能够正常编译。完成后，将生成的二进制文件加载到嵌入式系统。如果可能，请检查程序是否未崩溃而运行，尽管此时仍看不到输出。

如果程序似乎已正确构建并运行，请检查调试日志是否正常工作。将上一节中用于 "hello world " 程序的代码放入 `debug_log.cc` 的 `DebugLog()` 实现中。

实际的测试代码位于 [`tensorflow/lite/micro/micro_error_reporter_test.cc`在](https://oreil.ly/0jD00) ，如下所示：

```
int main(int argc, char** argv) {
  tflite::MicroErrorReporter micro_error_reporter;
  tflite::ErrorReporter* error_reporter = &micro_error_reporter;
  error_reporter->Report("Number: %d", 42);
  error_reporter->Report("Badly-formed format string %");
  error_reporter->Report("Another % badly-formed %% format string");
  error_reporter->Report("~~~%s~~~", "ALL TESTS PASSED");
}
```

它并非直接调用 `DebugLog()`，而是通过先处理可变参数等的 `ErrorReporter` 接口，但其基础实现依赖于刚刚编写的代码。如果一切正常工作，调试控制台应该会显示如下内容：

```
Number: 42
Badly-formed format string
Another  badly-formed  format string
~~~ALL TESTS PASSED~~~
```

完成后，希望将 `DebugLog()` 的实现移回主源树。为此，需要使用之前介绍的子文件夹专用技术。需要决定一个用于识别平台的简短名称（不包含大写字母、空格和其他特殊字符）。例如，已支持的平台使用 `arduino`、`sparkfun_edge`、`linux`。本教程中使用 `my_mcu`。从在 `TensorFlow/lite/micro/` 中创建新的子文件夹 `my_mcu` 开始，该文件夹是 GitHub 上签出的源代码副本（不是生成的或下载的）。将包含实现的 `debug_log.cc` 文件复制到该 `my_mcu` 文件夹中，并使用 Git 添加到源跟踪。将生成的项目文件复制到备份位置并执行以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile TARGET=my_mcu clean
make -f tensorflow/lite/micro/tools/make/Makefile \
  TARGET=my_mcu generate_micro_error_reporter_test_make_project
```

查看 [`tensorflow/lite/micro/tools/make/gen/my_mcu_x86_64/prj/micro_error_reporter_test/make/tensorflow/lite/micro/`查看](...) ，默认的 `debug_log.cc` 已不再存在，但实现位于 `my_mcu` 子文件夹中。将此源文件集拖回 IDE 或构建系统后，应该能够成功构建、运行并在调试控制台中显示输出。

## 执行所有目标

这样，TensorFlow 的测试目标和可执行目标都已生效！调试日志的实现是您需要进行的唯一平台特定更改。代码库中的其他所有内容都以足够可移植的方式编写，以便在支持 C++11 的工具链上构建和运行，而无需超出 `math` 库的标准库链接。要创建可在 IDE 中尝试的所有目标，请从终端执行以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile generate_projects \
  TARGET=my_mcu
```

这将在与生成的错误报告器测试类似的位置创建许多文件夹，每个文件夹练习库的不同部分。如果您想在平台上运行语音唤醒词示例，请查看 `tensorflow/lite/micro/tools/make/gen/my_mcu_x86_64/prj/micro_speech/make/`。

现在，`DebugLog()` 已实现并在平台上运行。但是，默认的 `audio_provider.cc` 实现始终返回一个全零数组，因此没有任何用处。要使其正常工作，需要使用前面介绍的子文件夹专用方法创建返回捕获音频的专用 `audio_provider.cc` 模块。如果您不坚持可运行的演示，也可以使用相同的示例代码来检查平台上神经网络的推理延迟等，或进行其他测试。

除了传感器和 LED 等输出设备的硬件支持之外，您可能还想利用平台的特殊功能来实现运行速度更快的神经网络运算符版本。我们欢迎此类专用优化，并希望如果证明子文件夹专用技术有用，则将其集成到主源树中将成为良好的方法。

## Makefile 构建与集成

对于许多嵌入式程序员来说，使用 IDE 比使用 `Make` 系统更简单、更熟悉。如果您想在持续集成构建中测试代码，或希望在特定 IDE 之外使用代码，则可能希望将更改与 `Makefile` 更完全地集成。为此所需的一项是发现可供下载的用于您平台的工具链，以及 SDK 和其他依赖项。例如，从 Arm 下载 `GCC Embedded` 工具链的 macOS 版和 Linux 版，并将其 URL 记录在 [`tensorflow/lite/micro/tools/make/third_party_downloads.inc`在](https://oreil.ly/WBrIy) 中。

然后，确定要传递给编译器和链接器的正确命令行标志，并将信息编码到 [`tensorflow/lite/micro/tools/make/targets`的](https://oreil.ly/zusVM) 子 `Makefile` 中，以防有子文件夹专用无法发现的额外必需源文件。如果您想要额外的功劳，请考虑使用 `Renode` 等工具在 x86 服务器上模拟微控制器，以便不仅在构建时验证，还能在持续集成期间运行测试。[`tensorflow/lite/micro/testing/test_bluepill_binary.sh`在](https://oreil.ly/A80CN) 中可以看到使用 `Renode` 测试运行 "Bluepill" 二进制文件的脚本示例。

如果所有构建集都正确设置，则可以执行如下操作来生成可刷写的二进制文件（根据平台设置适当的 target）：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  TARGET=bluepill micro_error_reporter_test_bin
```

如果用于运行测试的脚本和环境正常工作，则可以执行以下操作来运行平台的所有测试：

```
make -f tensorflow/lite/micro/tools/make/Makefile TARGET=bluepill test
```

# 支持新的 IDE 或构建系统

TensorFlow Lite for Microcontrollers 可以为 `Arduino`、`Mbed`、`Keil` 工具链创建独立项目，但我们也知道嵌入式工程师使用许多其他开发环境。如果您需要在新环境中运行框架，我们首先建议您检查是否可以将 `Make` 项目生成时生成的“原始”文件集导入 IDE。此类项目存档仅包含特定目标所需的源文件，包括第三方依赖项。

###### 注

当只有几个文件时，在导出到生成项目时将它们保留在原始源树的嵌套子文件夹（例如 `tensorflow/lite/micro/examples/micro_speech`）中似乎很奇怪。将目录层次结构 flatten 会更合理吗？

我们决定保持深度嵌套文件夹的原因是，尽管在处理生成的项目文件时稍显不便，但可以尽可能轻松地将其合并到主源树中。如果从 `GitHub` 签出的原始代码与每个项目中的副本的路径始终一致，则更改和更新的跟踪将变得容易得多。

遗憾的是，此方法并非适用于所有 IDE。例如，`Arduino` 库要求所有 C++ 源代码文件的后缀为 `.cpp`，而不是 TensorFlow 默认的 `.cc`。此外，也无法指定包含路径，因此在将原始文件复制到 `Arduino` 时需要更改代码中的路径。为了支持这种复杂的转换，我们为 `Makefile` 构建准备了一些规则和脚本。调用 [`generate_microlite_projects()`](https://oreil.ly/YYoHm) ，为每个 IDE 专用，并进一步依赖 [规则](https://oreil.ly/KHo7G)、[Python 脚本](https://oreil.ly/BKLhn)、[模板文件在](https://oreil.ly/tDFhh) 来创建最终输出。如果您需要在自己的 IDE 中执行类似操作，则需要使用 `Makefile` 添加类似功能。

# 在项目和存储库之间集成代码更改

代码生成系统的最大缺点之一是源的多个副本散布在不同位置，使得处理代码更新变得非常麻烦。为了将合并更改的成本降至最低，我们采用了一些惯例和推荐程序。最常见的用例是在项目本地副本中的文件进行更改后，更新到 TensorFlow Lite 框架的新版本以获取新增功能或错误修复。以下是针对这种情况的建议：

1. 下载为您的 IDE 和目标构建的项目文件的构建存档，或使用您感兴趣的框架版本从 `Makefile` 手动生成项目文件。
