

## 图 5-8. 添加一个新的 Thing

## 图 5-9. Arduino Thing 的仪表盘

## 添加属性

我们可以通过 Thing 属性来暴露 Arduino Nano 33 IoT 的 I/O。例如，我们想将传感器数据暴露给 Arduino IoT Cloud。在此场景中，我们想暴露 Arduino Nano 33 IoT 上的内置 LED。我们可以对 LED 设置 I/O，以便通过网络打开/关闭 LED。

你可以先在 Arduino IoT Cloud 上打开一个 Thing。图 5-9 显示了我们已经创建的 Thing。点击 Add Property 按钮添加一个新属性。点击后，你将获得一个表单，如图 5-10 所示。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_146_0.png)

图 5-10. 添加一个新属性

在此场景中，你可以设置以下选项：

- Name: LED1
- VARIABLE NAME: LED1
- TYPE: ON/OFF (Boolean)
- Permission: Read & Write
- Update: when the value changes
- History: checked

我的条目显示在图 5-10 中。填写所有字段后，你可以点击 ADD PROPERTY 按钮。你将返回 Things 表单。你应该在 Thing 表单上看到 LED1 属性，如图 5-11 所示。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_147_0.png)

图 5-11. 带有一个属性的 Thing

你已经创建了一个 Thing。接下来，我们可以修改 Sketch 程序以连接到此 LED1 属性。

## 编辑 Sketch 程序

现在我们可以编辑我们的 Sketch 程序。在 Thing 仪表盘的属性表单上（见图 5-11），你可以点击 EDIT SKETCH 按钮。点击后，你将获得 Arduino 网页编辑器，如图 5-12 所示。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_148_0.png)

*图 5-12.* 编辑 Sketch 程序

首先，我们修改 Secret 程序以配置 Arduino Nano 33 IoT 上的 WiFi 网络。在 Secret 选项卡上填写 SSID ID 和 SSID key。现在我们在主 Sketch 上修改代码。

你可以看到我们的属性变量 LED1 已在主 Sketch 程序中声明。

```
#include "arduino_secrets.h"
bool LED1;
#include "thingProperties.h"
```

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_150_0.png)

## 图 5-13. Arduino IoT Cloud 的仪表盘

我们可以在仪表盘编辑器上添加一个新的开关。点击 ADD 按钮并选择 Switch widget。你可以在图 5-14 中看到 Switch widget 选项。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_150_1.png)

## 图 5-14. 在仪表盘上添加一个小部件

点击 Switch widget 然后将其拖到仪表盘编辑器。拖动 Switch widget 后，你应该可以看到 Switch widget，如图 5-15 所示。

点击 Example Data 将 Switch widget 链接到 Thing 属性。点击后，你将获得如图 5-16 所示的表单。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_151_0.png)

图 5-15. 添加一个开关

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_152_0.png)

## 图 5-16. 设置 Switch widget

点击 Linked Property 按钮以链接到 Thing 属性。我们将获得一个表单，如图 5-17 所示。选择我们的 Thing 名称，并选择 Property (LED1)。如果完成，点击 LINK PROPERTY 按钮。然后，我们将返回到仪表盘编辑器。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_153_0.png)

*图 5-17.* 将开关链接到 thing 属性

你也可以重命名我们的仪表盘程序——例如，LED Demo。现在我们可以测试我们的仪表盘程序。

## 测试

要测试我们的程序，我们需要将仪表盘程序激活到运行模式。你可以点击 USE DASHBOARD 按钮来激活仪表盘程序。

你可以尝试将 Switch 切换到 ON 模式。然后，你应该看到 Arduino Nano 33 IoT 上的内置 LED 点亮。你也可以通过将 Switch 切换到 OFF 模式来关闭 LED。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_154_0.png)

*图 5-18. 仪表盘编辑器上的 LED 演示程序*

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_154_1.png)

*图 5-19. 切换 Switch 以打开 LED*

你可以通过添加一些 LED 来自定义此程序。然后，你可以在 Things 程序上添加一些属性。

## 开发传感器监控

我们已经创建了 Arduino IoT Cloud 来打开/关闭 Arduino Nano 33 IoT 上的 LED。现在我们可以继续构建 Arduino IoT Cloud 程序以监控传感器。在此演示中，我们使用 Arduino Nano 33 IoT 上的内置传感器。为测试，我们使用陀螺仪传感器。

要实现此演示，我们可以按以下步骤执行一些任务：

- adding a new thing
- adding properties
- editing the Sketch program
- building a dashboard
- testing

我们在下一节中实现这些步骤。

## 添加一个新的 Thing

你可以在 Arduino IoT Cloud 上创建一个新的 Thing。如果你有免费会员，你应该删除 Arduino IoT Cloud 上的现有 Thing，因为你只能创建一个 Thing。

现在你可以创建一个新的 Thing。例如，我们将 Thing 名称设为 GyroscopeThing。然后，我们可以向 GyroscopeThing 添加一些属性。

## 添加属性

创建 Thing 后，我们可以添加属性。对于此演示，我们创建三个属性来监控来自 Arduino Nano 33 IoT 的陀螺仪传感器。这些属性将链接到陀螺仪传感器的 X、Y 和 Z 度。当我们可以添加一个新属性时，我们有一个表单，如图 5-20 所示。我们添加三个属性，其属性参数如表 5-1 所示。

创建三个 Thing 属性后，我们将返回到 Things 仪表盘。你可以在图 5-21 中看到三个属性。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_157_0.png)

图 5-21. GyroscopeThing 上的三个属性

## 编辑 Sketch 程序

在 GyroscopeThing 上添加三个属性后，我们可以编辑 Sketch 程序。我们将读取陀螺仪传感器，然后将传感器数据更新到属性变量。

点击 EDIT SKETCH 按钮以在 Arduino IoT Cloud 上编辑我们的程序。然后，我们有 Sketch 网页编辑器。我们修改此 Sketch 程序以使我们能够读取陀螺仪传感器并更新三个 Thing 属性。

首先，我们在 Arduino Secret 上设置 SSID ID 和 SSID key。然后，我们打开主程序。我们在 Sketch 程序上添加 LSM6DS3 库。

```
#include "arduino_secrets.h"
#include <Arduino_LSM6DS3.h>
```

我们将看到我们的属性变量，例如 xDegree、yDegree 和 zDegree。

```
float xDegree;
float yDegree;
float zDegree;
```

```
#include "thingProperties.h"
```

在 setup() 函数中，我们通过调用 IMU.begin() API 初始化 LSM6DS3 库。然后，我们可以访问 Arduino Nano 33 IoT 上的陀螺仪传感器。

```
void setup() {
...
  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1);
  }
...
}
```

我们可以读取陀螺仪传感器然后更新到 xDegree、yDegree 和 zDegree 变量。我们可以调用 IMU.gyroscopeAvailable() 来检查传感器数据是否可用。要读取传感器数据，我们可以使用 IMU.readGyroscope() 函数。

```
void loop() {
    ArduinoCloud.update();
    // Your code here
    if (IMU.gyroscopeAvailable()) {
        IMU.readGyroscope(xDegree, yDegree, zDegree);
    }
    delay(1000);
}
```

保存此程序。现在你可以编译此 Sketch 程序并上传到 Arduino Nano 33 IoT 设备。

## 构建仪表盘

我们构建一个仪表盘以在 Arduino Nano 33 IoT 和 Arduino IoT Cloud 之间创建交互。我们按以下步骤执行一个新的仪表盘程序：

- Create a new dashboard.
- Add three value widgets into the dashboard editor.
- Each value widget is to be linked to each Thing Property, as shown in Figure 5-22.
- Do the same action for XDegree, YDegree, and ZDegree value widgets.

最后，我们可以设置仪表盘名称。现在我们可以测试仪表盘上的小部件。

![](img/62b0cd46c774c5bcb05b9b67bb2cb8b0_160_0.png)

图 5-22. 将属性链接到小部件

## 测试

我们可以测试我们的 Arduino IoT Cloud 程序。你可以导航到 Arduino IoT Cloud 仪表盘。点击我们已经创建的仪表盘表单。点击 USE DASHBOARD 按钮进入 RUN 模式。

你可以在图 5-23 中看到小部件输出。摇动你的 Arduino Nano 33 IoT 板以在仪表盘小部件上看到传感器数据变化。

