
Arduino 库可以从 [从 GitHub 获取](https://oreil.ly/93OKK)。要安装，请下载或克隆仓库。然后将 `ArduCAM` 子目录复制到 `Arduino/libraries` 目录。要找到机器上的 `libraries` 目录，请在 Arduino IDE 的 Preferences 窗口中检查 Sketchbook 的位置。

下载库后，需要编辑库文件以确保其配置为 Arducam Mini 2MP Plus。为此，请打开 `Arduino/libraries/ArduCAM/memorysaver.h`。

应该会显示 `#define` 列表。请确认除 `#define OV2640_MINI_2MP_PLUS` 外的所有内容都已注释掉，如下所示：

```
 //Step 1: select the hardware platform, only one at a time
 //#define OV2640_MINI_2MP
 //#define OV3640_MINI_3MP
 //#define OV5642_MINI_5MP
 //#define OV5642_MINI_5MP_BIT_ROTATION_FIXED
 #define OV2640_MINI_2MP_PLUS
 //#define OV5642_MINI_5MP_PLUS
 //#define OV5640_MINI_5MP_PLUS
```

保存文件后，Arducam 库的配置就完成了。

### 提示

本示例使用 Arducam 库的提交 #e216049 开发。如果在库中遇到问题，请下载此特定提交，以确保使用完全相同的代码。

下一步是安装 JPEGDecoder 库。这可以通过 Arduino IDE 完成。选择 `Tools` 菜单中的 `Manage Libraries` 选项，搜索 `JPEGDecoder`。安装库的版本 1.8.0。

安装库后，需要对其进行配置以禁用与 Arduino Nano 33 BLE Sense 不兼容的可选组件。打开 `Arduino/libraries/JPEGDecoder/src/User_Config.h`，并确认 `#define LOAD_SD_LIBRARY` 和 `#define LOAD_SDFAT_LIBRARY` 都已注释掉，如本文件摘录所示：

```
 // Comment out the next #defines if you are not using an SD Card to store
 // the JPEGs
 // Commenting out the line is NOT essential but will save some FLASH space if
 // SD Card access is not needed. Note: use of SdFat is currently untested!

 //#define LOAD_SD_LIBRARY // Default SD Card library
 //#define LOAD_SDFAT_LIBRARY // Use SdFat library instead, so SD Card SPI can
                              // be bit bashed
```

保存文件后，库的安装就完成了。现在可以准备运行人物检测应用程序了！

将 Arduino 设备通过 USB 连接。如下图所示 [图9-](#ch09.html#arduino_board_dropdown_9)3，请确认在工具菜单的板子下拉列表中选择了正确的设备类型。

![](assets/timl_0605.png)

如果设备名称未在列表中显示，则需要安装支持包。为此，请点击 `Boards Manager`。在出现的窗口中搜索设备，并安装相应支持包的最新版本。

在 `Tools` 菜单的 `Port` 下拉列表中，确认已选择设备的端口。

![](assets/timl_0606.png)

最后，在 Arduino 窗口中点击上传按钮（[图9-5中](#ch09.html#arduino_upload_button_9)以白色高亮显示），编译代码并上传到 Arduino 设备。

![](assets/timl_0607.png)

上传成功完成后，程序会立即运行。

要测试，首先将设备的摄像头对准非人物体或遮住镜头。接下来，当蓝色 LED 闪烁时，设备会从摄像头捕获帧并开始推理。用于人物检测的推理模型相对较大，因此推理需要较长时间。截至撰写时约为 19 秒，但此后 TensorFlow Lite 可能已得到加速。

推理完成后，其结果会转换为另一个 LED 点亮。由于摄像头对准的是非人物体，因此红色 LED 应该会点亮。

现在，让我们将设备的摄像头对准自己！当蓝色 LED 下次闪烁时，设备会捕获另一张图像并开始推理。大约 19 秒后，绿色 LED 应该会点亮。

请记住，图像数据在每次推理之前都会在蓝色 LED 闪烁时作为快照捕获。此时摄像头所对准的内容将被输入模型。在下一次图像被捕获、蓝色 LED 再次闪烁之前，摄像头指向何处并不重要。

如果出现看似不正确的结果，请确认处于光线良好的环境中。还需检查摄像头的方向是否正确、引脚是否向下、拍摄的图像方向是否正确。此外，请记住这是一个以精度换取小巧的小型模型。它工作得非常好，但并不总是 100% 准确。

推理结果也可以在 Arduino Serial Monitor 中查看。为此，请从工具菜单打开串行监视器。会显示详细日志，说明应用程序运行期间发生了什么。此外，如果在 "Show timestamp " 框中打勾，可以查看每个过程所需的时间：

```
14:17:50.714 -> Starting capture
14:17:50.714 -> Image captured
14:17:50.784 -> Reading 3080 bytes from ArduCAM
14:17:50.887 -> Finished reading
14:17:50.887 -> Decoding JPEG and converting to greyscale
14:17:51.074 -> Image decoded and processed
14:18:09.710 -> Person score: 246 No person score: 66
```

从此日志可以看出，摄像头模块图像数据的捕获和读取约需 170 毫秒，JPEG 解码和转换为灰度约需 180 毫秒，推理执行约需 18.6 秒。

### 自行修改

在部署了基本应用程序后，让我们尝试修改代码。编辑并保存 Arduino IDE 中的文件，然后重复之前的步骤将修改后的代码部署到设备上。

有几件事可以尝试：

*   修改检测响应器，使其忽略“人”和“无人”分数差异不大的模糊输入。
*   使用人物检测结果控制其他组件，如额外的 LED 或舵机。
*   构建智能安全摄像头，保存或发送图像。

## SparkFun Edge

SparkFun Edge 板针对低功耗进行了优化。与同样高效的摄像头模块结合使用时，它成为构建电池供电推理应用程序的理想平台。通过板上的排线电缆适配器连接摄像头模块非常简单。

### 购买哪个摄像头模块

本示例使用 SparkFun 公司的 [Himax HM01B0 breakout 摄像头模块](https://oreil.ly/H24xS)。它基于 320×320 像素的图像传感器，功耗极低：在 30 帧/秒（FPS）下捕获时，功耗低于 2mW。

### 在 SparkFun Edge 上捕获图像

要使用 Himax HM01B0 摄像头模块开始捕获图像，首先需要初始化摄像头。完成后，每当需要新图像时都可以从摄像头读取帧。帧是表示摄像头当前所能看到内容的字节数组。

对于此摄像头，将大量使用作为构建过程一部分下载的 Ambiq Apollo3 SDK 以及位于 `sparkfun_edge/himax_driver` 中的 HM01B0 驱动程序。

图像提供程序实现在 ` sparkfun_edge/image_provider.cc` 中。此代码特定于 SparkFun 板和 Himax 摄像头模块，因此不详细介绍。相反，让我们逐步说明高层发生了什么。

`GetImage()` 函数是图像提供程序与世界的接口。它在应用程序的主循环中被调用，以获取图像数据的帧。首次调用时，需要初始化摄像头。这通过调用 `InitCamera()` 函数来完成：

```
 // Capture single frame.  Frame pointer passed in to reduce memory usage.  This
 // allows the input tensor to be used instead of requiring an extra copy.
 TfLiteStatus GetImage(tflite::ErrorReporter* error_reporter, int frame_width,
                       int frame_height, int channels, uint8_t* frame) {
   if (!g_is_camera_initialized) {
     TfLiteStatus init_status = InitCamera(error_reporter);
     if (init_status != kTfLiteOk) {
       am_hal_gpio_output_set(AM_BSP_GPIO_LED_RED);
       return init_status;
     }
```

如果 `InitCamera()` 返回除 `kTfLiteOk` 之外的状态，则会点亮板上的红色 LED（使用 `am_hal_gpio_output_set(AM_BSP_GPIO_LED_RED)`）以指示问题。这有助于调试。

`InitCamera()` 函数在 `image_provider.cc` 中定义。此函数非常特定于设备，因此此处不作说明。如果想在自己的代码中使用，可以直接复制粘贴。

调用 Apollo3 SDK 的函数，设置微控制器的输入输出，使其能够与摄像头模块通信。还启用摄像头用于发送新图像数据的机制——中断。一旦所有设置完成，就使用摄像头驱动程序打开摄像头的电源并将其配置为持续捕获图像。

摄像头模块具有自动曝光功能，帧被捕获时会自动校准曝光设置。为了在推理前进行校准，`GetImage()` 函数的下一部分使用摄像头驱动程序的 `hm01b0_blocking_read_oneframe_scaled()` 函数捕获一些帧。捕获的数据不做任何处理。这只是为了给摄像头模块的自动曝光功能提供处理材料：

```
     // Drop a few frames until auto exposure is calibrated.
     for (int i = 0; i < kFramesToInitialize; ++i) {
       hm01b0_blocking_read_oneframe_scaled(frame, frame_width, frame_height,
                                            channels);
     }
     g_is_camera_initialized = true;
   }
```

设置完成后，只需调用 `GetImage()` 中的 `hm01b0_blocking_read_oneframe_scaled()` 来捕获图像：

```
 hm01b0_blocking_read_oneframe_scaled(frame, frame_width, frame_height,
                                      channels);
```

在应用程序的主循环中调用 `GetImage()` 时，`frame` 变量是指向输入张量的指针，因此数据会由摄像头驱动程序直接写入分配给输入张量的内存区域。还指定了宽度、高度和通道数。

通过此实现，可以从摄像头模块获取图像数据。接下来，让我们看看如何对模型的输出做出反应。

### 在 SparkFun Edge 上响应检测

检测响应器的实现与唤醒词示例中的命令响应器非常相似。每次执行推理时，设备会切换蓝色 LED。检测到人时绿色 LED 点亮，未检测到人时黄色 LED 点亮。

实现位于 ` sparkfun_edge/detection_responder.cc`。让我们大致看一下。

`RespondToDetection()` 函数接收“人”和“非人”两个分数。首次调用时，设置蓝色、绿色和黄色 LED 为输出：

```
 void RespondToDetection(tflite::ErrorReporter* error_reporter,
                         uint8_t person_score, uint8_t no_person_score) {
   static bool is_initialized = false;
   if (!is_initialized) {
     // Setup LED's as outputs.  Leave red LED alone since that's an error
     // indicator for sparkfun_edge in image_provider.
     am_hal_gpio_pinconfig(AM_BSP_GPIO_LED_BLUE, g_AM_HAL_GPIO_OUTPUT_12);
     am_hal_gpio_pinconfig(AM_BSP_GPIO_LED_GREEN, g_AM_HAL_GPIO_OUTPUT_12);
     am_hal_gpio_pinconfig(AM_BSP_GPIO_LED_YELLOW, g_AM_HAL_GPIO_OUTPUT_12);
     is_initialized = true;
   }
```

此函数每次推理调用一次，因此以下代码片段会在每次执行推理时切换蓝色 LED 的点亮和熄灭：

```
 // Toggle the blue LED every time an inference is performed.
 static int count = 0;
 if (++count & 1) {
     am_hal_gpio_output_set(AM_BSP_GPIO_LED_BLUE);
 } else {
     am_hal_gpio_output_clear(AM_BSP_GPIO_LED_BLUE);
 }
```

最后，如果检测到人，则点亮绿色 LED；如果未检测到，则点亮蓝色 LED。此外，使用 `ErrorReporter` 实例记录分数：
