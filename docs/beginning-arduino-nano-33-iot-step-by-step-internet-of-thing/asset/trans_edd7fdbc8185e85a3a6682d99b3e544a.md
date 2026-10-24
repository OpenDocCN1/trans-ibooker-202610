

一旦我们建立了客户端连接，就需要等待传入的 HTTP 请求包。我们解析数据包以识别命令类型。可以使用字符串处理过程。当收到“/gpioX/Y”请求时，我们调用 `indexOf()`。X 是 LED 编号，Y 是打开/关闭 LED 的命令。

```cpp
String req = client.readStringUntil('\r');
Serial.println(req);
client.flush();

// Match the request
int val1 = 0;
int val2 = 0;
int val3 = 0;
int ledreq = 0;
if (req.indexOf("/gpio1/0") != -1) {
    val1 = 0;
    ledreq = 1;
}
else if (req.indexOf("/gpio1/1") != -1) {
    val1 = 1;
    ledreq = 1;
}
else if (req.indexOf("/gpio2/0") != -1) {
    val2 = 0;
    ledreq = 2;
}
else if (req.indexOf("/gpio2/1") != -1) {
    val2 = 1;
    ledreq = 2;
}
else if (req.indexOf("/gpio3/0") != -1) {
    val3 = 0;
    ledreq = 3;
}
else if (req.indexOf("/gpio3/1") != -1) {
    val3 = 1;
    ledreq = 3;
}
else {
    Serial.println("invalid request");
    client.stop();
    return;
}
```

识别命令类型后，我们可以使用 `digitalWrite()` 打开/关闭 LED。

```cpp
digitalWrite(led1, val1);
digitalWrite(led2, val2);
digitalWrite(led3, val3);
```

接下来，我们向客户端发送响应。我们使用 `client.print()` 函数发送 HTML 脚本。之后，我们关闭客户端连接。

```cpp
client.flush();
String s = "HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n<!DOCTYPE HTML>\r\n<html>\r\n";
if(ledreq==1) {
    s += "LED1 is ";
    s += (val1)? "ON": "OFF";
}else if(ledreq==2) {
    s += "LED2 is ";
    s += (val2)? "ON": "OFF";
}else if(ledreq==3) {
    s += "LED3 is ";
    s += (val3)? "ON": "OFF";
}
s += "</html>\n";

// Send the response to the client
client.print(s);
delay(1);
client.stop();
Serial.println("Client disconnected");
}
```

你可以将此程序保存为 IoTDemo。接下来，我们可以编译并测试该程序。

## 测试

我们将 IoTDemo 程序编译并上传到 Arduino Nano 33 IoT 后，可以打开串行监视器工具。我们应该会看到 Arduino Nano 33 IoT 的 IP 地址。如果看不到，说明 Arduino Nano 33 IoT 在连接现有 WiFi 时可能遇到了问题。图 4-9 显示了 Arduino Nano 33 IoT 的 IP 地址。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_134_0.png)

图4-9. Arduino Nano 33 IoT 已连接到 WiFi

测试时，我们可以使用浏览器。你可以发出一个打开 LED 的请求。我们可以访问 http://<ipaddress>/gpio1/1 来打开 LED 1。如果成功，我们会得到如图 4-10 所示的响应。LED 1 也会点亮。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_135_0.png)

图4-10. 通过 HTTP GET 请求打开 LED

要关闭 LED 1，我们可以使用此链接发出请求：http://<ipaddress>/gpio1/0。我们会看到 LED 1 熄灭。同时也会收到来自 Arduino Nano 33 IoT 的响应，如图 4-11 所示。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_135_1.png)

图4-11. 通过 HTTP GET 请求关闭 LED

我们的 IoTDemo 程序还会打印有关客户端连接和客户端请求的所有信息。你可以在图 4-12 中看到我的程序输出。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_136_0.png)

图4-12. 串行终端上 IoT 项目的程序输出

本章到此结束。我们可以通过基于 Arduino Nano 33 IoT 板开发 IoT 程序来进行练习。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_136_0.png)

图4-12. 串行终端上 IoT 项目的程序输出

## 总结

我们已经学习了如何在 Arduino Nano 33 IoT 上设置 WiFiNINA 库。我们将开发板连接到了现有的 WiFi 网络。然后，我们尝试连接到 Google 网站。我们从 NTP 服务器请求了当前时间。最后，我们构建了一个用于打开/关闭 LED 的 IoT 应用程序。

在下一章中，我们将学习如何连接到 Arduino IoT Cloud。

## 设置 Arduino IoT Cloud

为了访问 Arduino IoT Cloud 并为其构建程序，我们需要设置 Arduino 设备。我们应在该平台注册一个新账户。你可以通过此链接在 Arduino IoT Cloud 上注册账户：https://create.arduino.cc/iot/。注册完成后，我们将 Arduino Nano 33 IoT 注册到 Arduino IoT Cloud。

你可以在图 5-1 中看到 Arduino IoT Cloud 的仪表板。我们有三个菜单：Things、Dashboards 和 Devices。没有付费订阅的成员只能在该平台创建 Things 菜单。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_138_0.png)

图5-1. Arduino IoT Cloud 网站的仪表板

如果你在 Arduino 商店有账户，可以使用同一账户注册 Arduino IoT Cloud。然后，我们就可以注册我们的 Arduino Nano 33 IoT。

### 注册 Arduino Nano 33 IoT

在使用 Arduino IoT Cloud 之前，我们必须注册 Arduino 设备。本章我们使用 Arduino Nano 33 IoT。注册 Arduino 设备需要一些步骤。你可以执行以下任务：

- 安装 Arduino Create Agent
- 添加新的 Arduino 设备

接下来，我们可以安装 Arduino Create Agent。

### 安装 Arduino Create Agent

Arduino Create Agent 是一个后台程序，用于监听本地 Arduino 设备。该代理程序充当本地 Arduino 设备与 Arduino IoT Cloud 之间的桥梁。

Arduino Create Agent 程序适用于 Windows、Linux 和 macOS 平台。你可以通过此链接下载该程序：https://github.com/arduino/arduino-create-agent。安装后，应允许该程序在后台运行。在 Windows 平台上，你可以在任务栏上看到 Arduino Create Agent 的托盘图标，如图 5-2 所示。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_139_0.png)

图5-2. Windows 操作系统上 Arduino Create Agent 的托盘图标

你可以通过在 Arduino Create Agent 中单击 Debug Console 菜单来打开它。如果 Arduino 设备已连接到本地计算机，Arduino Create Agent 的调试控制台将显示设备状态。图 5-3 显示我的 Arduino Nano 33 IoT 已被检测到。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_140_0.png)

图5-3. Arduino Create Agent 调试控制台的界面

如果你没有看到 Arduino 设备，请确保 Arduino 已正确连接到计算机。完成后，你可以注册 Arduino Nano 33 IoT。

### 添加新的 Arduino 设备

一旦我们在本地计算机上设置了 Arduino Create Agent 程序，就可以添加新的 Arduino 设备。打开 Arduino IoT Cloud 网站，然后点击 DEVICES 菜单。你会看到 Arduino 设备列表。

现在你可以添加新的 Arduino Nano 33 IoT。插入 Arduino Nano 33 IoT 后，你应该会看到如图 5-4 所示的对话框。点击 Arduino 设备菜单中的 Set up。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_141_0.png)

图5-4. 添加新的 Arduino 设备

点击此选项后，你应该会看到 Arduino Nano 33 IoT，如图 5-5 所示。如果没有看到 Arduino Nano 33 IoT，请确保 Arduino Nano 33 IoT 已连接并且 Arduino Create Agent 正在运行。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_142_0.png)

图5-5. Arduino IoT Cloud 检测到 Arduino Nano 33 IoT

为 Arduino Nano 33 IoT 设置设备名称。完成后，可以点击 CONFIGURE 按钮。Arduino IoT Cloud 将配置你的 Arduino Nano 33 IoT，如图 5-6 所示。完成此任务需要几分钟时间。配置完成后，你应该会在 Arduino IoT Cloud 列表中看到 Arduino Nano 33 IoT，如图 5-7 所示。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_143_0.png)

图5-6. 正在配置 Arduino Nano 33 IoT

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_143_1.png)

图5-7. Arduino Nano 33 IoT 设备已添加

## 开发远程 LED 按钮

在本节中，我们将构建一个程序，通过 Arduino IoT Cloud 仪表板远程控制 LED。我们可以从 Arduino IoT Cloud 仪表板网站打开/关闭 Arduino Nano 33 IoT 上的 LED。

为了实现演示，我们可以按以下步骤执行一些任务：

- 添加新的 Thing
- 添加属性
- 编辑 Sketch 程序
- 构建仪表板
- 测试

我们将在下一节中执行这些步骤。

### 添加新的 Thing

Arduino IoT Cloud 的一个功能是与 Arduino 设备交互的程序。使用免费会员资格，我们只能创建一个 Thing。在本演示中，我们通过 Arduino IoT Cloud 仪表板构建 LED 的遥控器。

打开 Arduino IoT Cloud 网站，点击 Things 菜单。然后，你会得到一个表单，如图 5-8 所示。输入 Thing 名称和 Arduino 设备。每个 Thing 都通过一个 Arduino 设备连接。

设置 Thing 名称后，你可以点击 CREATE 按钮。之后，你会看到一个表单，如图 5-9 所示。我们将在下一节配置 Arduino Nano 33 IoT 的属性。

