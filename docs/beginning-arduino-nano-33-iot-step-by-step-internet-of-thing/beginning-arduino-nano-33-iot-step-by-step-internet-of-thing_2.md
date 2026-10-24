# CHAPTER 1
### 设置开发环境

Arduino Nano 33 IoT 是一种物联网（IoT）解决方案，用于在物理环境中进行传感和执行。Arduino Nano 33 IoT 板配备了 WiFi 和 BLE 模块，可与其他实体进行通信以交换数据。本章将探讨如何为开发设置 Arduino Nano 33 IoT 板。

本章的主题列表如下：

- Reviewing Arduino Nano 33 IoT board
- Setting up development environment
- Building LED blinking program
- Applying Arduino web editor

### Introduction

Arduino Nano 33 IoT 是 Arduino 的物联网平台之一。该板使用 WiFi 和蓝牙模块连接到网络。WiFi 是人们用来访问互联网的常见网络。蓝牙是无线个人局域网（WPAN）的一部分，可在短距离内与其他设备通信。

Arduino Nano 33 IoT 板专为低成本物联网设备设计，以解决您的物联网问题。Arduino Nano 33 IoT 尺寸小巧，45 x 18 mm（长 x 宽）。您可以在图 1-1 中看到我的 Arduino Nano 33 IoT 板。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_10_0.png)

Figure 1-1. Arduino Nano 33 IoT board

### Review Arduino Nano 33 IoT Board

Arduino Nano 33 IoT 由 ARM Cortex M0 32 位 SAMD21 构建。该板还具有来自 u-blox 的射频模块 NINA-W102。该模块专为 WiFi 和蓝牙数据通信而设计。您可以在表 1-1 中阅读 Arduino Nano 33 IoT 的详细规格。

由于 Arduino Nano 33 IoT 具有一些数字和模拟 I/O，我们通过与其他传感器或执行器连接来扩展板的能力。我们还使用通用异步收发器/发送器（UART）、串行外设接口（SPI）和内部集成电路（I2C）协议与其他设备通信。

**Table 1-1.** *A Specification of Arduino Nano 33 IoT*

| Features | Notes |
|---|---|
| Microcontroller | SAMD21 Cortex-M0+ 32-bit |
| Radio module | u-blox NINA-W102 |
| Secure module | ATECC608A |
| Operating voltage | 3.3V |
| Input voltage | 21V |
| DC current per I/O pin (limit) | 7 mA |
| Clock speed | 48 Mhz |
| CPU flash memory | 256 KB |
| SRAM | 32 KB |
| EEPROM | None |
| Digital I/O | 14 |
| PWM pins | 11 (2, 3, 5, 6, 9, 10, 11, 12, 16 / A2, 17 / A3, 19 / A5) |
| UART | 1 |
| SPI | 1 |
| I2C | 1 |
| Analog Input | 8 (ADC 8/10/12 bit) |
| Analog Output | 1 (DAC 10 bit) |
| LED_BUILTIN | 13 |
| USB | Native in the SAMD21 processor |
| IMU | LSM6DS3 |
| Size (Length x Width) | 45 mm x 18 mm |

Key: CPU, central processing unit; SRAM, static random-access memory; EEPROM, electrically erasable programmable read-only memory; PWM, pulse width modulation; UART, universal asynchronous receiver/transmitter; SPI, serial peripheral interface; I2C, inter-integrated circuit; USB, universal serial bus; IMU, inertial measurement unit.

接下来，我们将在计算机上设置 Arduino Nano 33 IoT，以便您可以为 Arduino 板构建程序。

### Set Up Development Environment

Arduino 提供软件来为所有 Arduino 板型号构建程序。我们可以使用 Arduino 软件。您可以在以下链接下载 Arduino 软件：https://www.arduino.cc/en/Main/Software。该软件适用于 Windows、Linux 和 macOS。

安装过程步骤简单。只需按照 Arduino 安装指南进行操作。安装完成后，您将在操作系统的主菜单中看到 Arduino 应用程序菜单。

打开 Arduino 应用程序。然后，我们将获得如图 1-2 所示的 Arduino 应用程序。您将在应用程序对话框中看到骨架代码。以下是一个代码模板。

```
void setup() {
    // put your setup code here, to run once:
}

void loop() {
    // put your main code here, to run repeatedly:
}
```

我们可以看出 Arduino 程序采用 C/C++ 程序语言方言。我们可以将所有数据初始化放在 setup() 函数中。程序将连续执行 loop() 函数内的代码。

# CHAPTER 1  SETTING UP DEVELOPMENT ENVIRONMENT

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_15_0.png)

*Figure 1-3.  Adding supported boards for Arduino Nano 33 IoT*

此安装需要几分钟才能完成。安装完成后，您可以在目标板上看到 Arduino Nano 33 IoT 板。您可以通过在 Arduino 软件中点击菜单 **Tools > Board ... > Boards Manager...** 来验证。您将看到您的板列表。图 1-4 显示了 Arduino 软件中的 Arduino Nano 33 IoT。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_17_0.png)

# Figure 1-5. Detected Arduino Nano 33 IoT on Device Manager—Windows 10

如果您在 Linux 上工作，您可以使用终端上的此命令验证 Arduino Nano 33 IoT。

```
$ ls /dev/ttyUSB*
```

您将看到通过 USB 连接的设备列表。Arduino Nano 33 IoT 通常检测为 /dev/ttyUSB0 或 /dev/ttyUSB1。对于 macOS，您可以输入此命令来检查 Arduino Nano 33 IoT。

```
$ ls /dev/cu*
```

您应该在终端上看到 USB 设备。

### Hello Arduino: Blinking LED

我们首先构建一个 Arduino 程序。Arduino Nano 33 IoT 板具有一个内置 LED，连接到数字引脚 13。在本节中，我们构建一个简单的 LED 闪烁。现在您可以将 Arduino Nano 33 IoT 连接到计算机。然后，我们可以开始编写 Arduino 程序。

您可以打开 Arduino 软件。我们从项目模板创建一个程序。您可以点击菜单，然后 File ➤ Examples ➤ 01.Basics ➤ Blink。点击后，您将获得如图 1-6 所示的程序代码。这是 Arduino 的示例程序。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_18_0.png)

*Figure 1-6.* Blink application on Arduino software

# CHAPTER 1   SETTING UP DEVELOPMENT ENVIRONMENT

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_20_0.png)

*Figure 1-7.* Compiling and flashing a program

将 Arduino 程序上传到 Arduino Nano 33 IoT 后，我们将在 Arduino Nano 33 IoT 板上看到闪烁的 LED。您可以在图 1-8 中看到我的闪烁 LED。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_21_0.png)

*Figure 1-8.* Blinking LED on Arduino Nano 33 IoT

### How does it work?

Arduino Nano 33 IoT 板在数字引脚 13 上有一个内置 LED。在我们的程序中，我们使用 pinMode() 将数字引脚 13 设置为数字输出。我们在 setup() 函数中初始化此数据。

```cpp
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}
```

Arduino 程序为内置 LED 引脚定义了 LED_BUILTIN。我们可以通过赋予值 OUTPUT 将引脚设置为输出模式。现在我们的程序将在 loop() 函数中连续运行。我们打开 LED 然后关闭 LED。我们可以使用 digitalWrite() 来执行 LED 的开/关。将值设置为 HIGH 以打开 LED。否则，我们可以通过在 digitalWrite() 函数中发送 LOW 值来关闭 LED。我们还为打开/关闭 LED 设置延迟。我们在 delay() 函数中设置 1000 ms。





```
void loop() {
    digitalWrite(LED_BUILTIN, HIGH);    // turn the LED on (HIGH
                                    is the voltage level)
    delay(1000);                        // wait for a second
    digitalWrite(LED_BUILTIN, LOW);     // turn the LED off by
                                    making the voltage LOW
    delay(1000);                        // wait for a second
}
```

你可以练习闪烁 LED 程序。

接下来，我们可以使用 Arduino 网页编辑器作为 Arduino 开发的替代工具。我们只需要一个浏览器和网络连接。

## Arduino Web Editor

Arduino 提供了一个在线编辑器来构建 Arduino 程序。在线编辑器的优势在于我们不需要准备太多运行环境和工具。我们只需要浏览器和网络连接即可。

我们可以使用任何浏览器访问 Arduino 网页编辑器。你可以导航到链接 https://create.arduino.cc/editor。图 1-9 显示了 Arduino 网页编辑器模型。要使用 Arduino 网页编辑器，我们必须在 Arduino 门户注册以构建 Arduino 程序。

在本节中，我们将重点介绍如何开始使用 Arduino 网页编辑器。我们将执行以下任务来完成使用在线网页编辑器的 Arduino 开发：

- 注册你的 Arduino 门户账户
- 安装 Arduino 插件
- 为 Arduino Nano 33 IoT 构建 blink 应用

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_23_0.png)

图 1-9. Arduino 网页编辑器

## 注册 Arduino 账户

要使用 Arduino 网页编辑器使用和构建 Arduino 程序，我们必须注册一个 Arduino 账户。此账户与在 Arduino 商店购买 Arduino 板时使用的账户类似。

你可以在右上角菜单图标上注册新的 Arduino 账户。你可以通过此门户填写个人信息。完成账户注册后，我们就可以使用 Arduino 网页编辑器构建 Arduino 程序。

## 安装 Arduino 插件

为了使我们的 Arduino Nano 33 IoT 能够连接到 Arduino 网页编辑器，我们需要安装 Arduino 插件。这是 Windows 所需的任务。Arduino 插件将充当本地 Arduino Nano 33 IoT 与 Arduino 网页编辑器之间的桥梁。

# CHAPTER 1　SETTING UP DEVELOPMENT ENVIRONMENT

首先，我们打开浏览器并导航到链接 https://create.arduino.cc/getting-started/plugin/welcome。然后，我们会看到一个表单，如图 1-10 所示。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_24_0.png)

**Figure 1-10.** *Arduino plug-in installation*

点击 START 按钮。之后，你会看到一个表单，如图 1-11 所示。点击 DOWNLOAD 按钮下载 Arduino 插件应用程序。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_26_0.png)

# Figure 1-12. Detecting the Arduino plug-in

点击 NEXT 按钮后，我们会收到安装完成的确认，如图 1-13 所示。

你可以点击 GO TO WEB EDITORS 按钮继续。你将被引导至 Arduino 网页编辑器，如图 1-9 所示。

现在我们已准备好使用 Arduino 网页编辑器进行 Arduino 开发。接下来，我们将构建一个 blink Arduino 应用。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_27_0.png)

图 1-13. 已完成 Arduino 插件安装

## 构建 Arduino 程序

Arduino 网页编辑器具有与桌面版 Arduino 软件相同的功能。Arduino 网页编辑器有项目示例。我们也可以向项目中添加 Arduino 库。

在本节中，我们像之前的项目一样构建一个 blink Arduino 应用。我们首先打开浏览器并导航到 https://create.arduino.cc/editor。点击左侧菜单中的 Examples 菜单。然后，点击 BUILTIN 选项卡并选择 01.BASICS(6) -> Blink。你可以在图 1-14 中看到我们的 Arduino 项目。

选择 Blink 项目示例后，我们就有了 blink 程序。你可以在图 1-15 中看到此程序。现在我们可以编译并将程序上传到 Arduino Nano 33 IoT。

从设备列表的下拉菜单中选择你的 Arduino Nano 33 IoT 板。点击下拉菜单左侧的 Verify and Upload 图标。此工具将编译并将 Arduino 程序上传到目标板。

# CHAPTER 1 SETTING UP DEVELOPMENT ENVIRONMENT

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_28_0.png)

图 1-14. 创建新项目

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_28_1.png)

图 1-15. 将程序上传到 Arduino Nano 33 IoT

我们可以使用 Arduino 网页编辑器尝试构建另一个 Arduino 项目。我们可以使用此工具的项目示例。

这是设置 Arduino 开发环境的章节结尾。

## Summary

我们学习了如何设置 Arduino 开发环境。我们还在桌面环境中安装了 Arduino 软件。我们构建了一个简单的 Arduino 程序 blink。此外，我们尝试使用 Arduino 网页编辑器构建 Arduino 程序。

接下来，我们将学习如何访问 Arduino Nano 33 IoT 输入/输出。我们也使用其他通信协议。
