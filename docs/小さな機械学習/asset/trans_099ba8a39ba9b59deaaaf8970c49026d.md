
```
void setup() {
  // Set up logging. Google style is to avoid globals or statics because of
  // lifetime uncertainty, but since this has a trivial destructor it's okay.
  static tflite::MicroErrorReporter micro_error_reporter; //NOLINT
  error_reporter = &micro_error_reporter;

  // Map the model into a usable data structure. This doesn't involve any
  // copying or parsing, it's a very lightweight operation.
  model = tflite::GetModel(g_magic_wand_model_data);
  if (model->version() != TFLITE_SCHEMA_VERSION) {
    error_reporter->Report(
        "Model provided is schema version %d not equal "
        "to supported version %d.",
        model->version(), TFLITE_SCHEMA_VERSION);
    return;
  }

  // Pull in only the operation implementations we need.
  // This relies on a complete list of all the ops needed by this graph.
  // An easier approach is to just use the AllOpsResolver, but this will
  // incur some penalty in code space for op implementations that are not
  // needed by this graph.
  static tflite::MicroMutableOpResolver micro_mutable_op_resolver; // NOLINT
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_DEPTHWISE_CONV_2D,
      tflite::ops::micro::Register_DEPTHWISE_CONV_2D());
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_MAX_POOL_2D,
      tflite::ops::micro::Register_MAX_POOL_2D());
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_CONV_2D,
      tflite::ops::micro::Register_CONV_2D());
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_FULLY_CONNECTED,
      tflite::ops::micro::Register_FULLY_CONNECTED());
  micro_mutable_op_resolver.AddBuiltin(tflite::BuiltinOperator_SOFTMAX,
                                       tflite::ops::micro::Register_SOFTMAX());

  // Build an interpreter to run the model with
  static tflite::MicroInterpreter static_interpreter(model,
                                                     micro_mutable_op_resolver,
                                                     tensor_arena,
                                                     kTensorArenaSize,
                                                     error_reporter);
  interpreter = &static_interpreter;

  // Allocate memory from the tensor_arena for the model's tensors
  interpreter->AllocateTensors();

  // Obtain pointer to the model's input tensor
  model_input = interpreter->input(0);
  if ((model_input->dims->size != 4) || (model_input->dims->data[0] != 1) ||
      (model_input->dims->data[1] != 128) ||
      (model_input->dims->data[2] != kChannelNumber) ||
      (model_input->type != kTfLiteFloat32)) {
    error_reporter->Report("Bad input tensor parameters in model");
    return;
  }

  input_length = model_input->bytes / sizeof(float);

  TfLiteStatus setup_status = SetupAccelerometer(error_reporter);
  if (setup_status != kTfLiteOk) {
    error_reporter->Report("Set up failed\n");
  }
}
```

更有趣的是在 `loop()` 函数中发生的事情：

```
void loop() {
  // Attempt to read new data from the accelerometer
  bool got_data = ReadAccelerometer(error_reporter, model_input->data.f,
                                    input_length, should_clear_buffer);
  // Don't try to clear the buffer again
  should_clear_buffer = false;
  // If there was no new data, wait until next time
  if (!got_data) return;
  // Run inference, and report any error
  TfLiteStatus invoke_status = interpreter->Invoke();
  if (invoke_status != kTfLiteOk) {
    error_reporter->Report("Invoke failed on index: %d\n", begin_index);
    return;
  }
  // Analyze the results to obtain a prediction
  int gesture_index = PredictGesture(interpreter->output(0)->data.f);
  // Clear the buffer next time we read data
  should_clear_buffer = gesture_index < 3;
  // Produce an output
  HandleOutput(error_reporter, gesture_index);
}
```

首先，尝试从加速度传感器读取值。尝试后，将 `should_clear_buffer` 设置为 `false`，暂时停止尝试清除。

如果获取新数据失败，`ReadAccelerometer()` 会返回 `false` 的值，并从 `loop()` 函数返回，以便下次调用时可以重试。

如果 `ReadAccelerometer()` 返回的值为 `true`，则使用刚刚输入的输入张量执行推理。将结果传递给 `PredictGesture()` 后，会得到检测到的手势的索引。如果索引小于 `3`，则该手势有效，因此设置 `should_clear_buffer` 标志，以便下次调用 `ReadAccelerometer()` 时清除缓冲区。然后调用 `HandleOutput()` 向用户报告结果。

在 `main.cc` 中，`main()` 函数启动程序，执行 `setup()` 函数，并循环调用 `loop()` 函数：

```
int main(int argc, char* argv[]) {
  setup();
  while (true) {
    loop();
  }
}
```

就这样结束了！要在开发计算机上构建程序，请使用以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile magic_wand
```

然后，要运行程序，请输入如下内容：

```
./tensorflow/lite/micro/tools/make/gen/osx_x86_64/bin/magic_wand
```

由于没有加速度传感器数据，程序不会生成任何输出，但可以确认构建和运行。

接下来，将说明获取加速度传感器数据并生成输出的各平台代码。也将介绍如何部署和运行应用程序。

# 部署到微控制器

本节将代码部署到两个设备：

*   [Arduino Nano 33 BLE Sense](https://oreil.ly/6qlMD)

*   [SparkFun Edge](https://oreil.ly/-hoL-)

让我们从 Arduino 实现开始。

## Arduino

Arduino Nano 33 BLE Sense 支持三轴加速度传感器和蓝牙，是制作魔杖的理想小型轻量设备。

让我们看看应用程序主要文件的 Arduino 特定实现。

### Arduino 常量

常量 `kConsecutiveInferenceThresholds` 在 [`arduino/constants.cc`](https://oreil.ly/5bBt0) 文件中被重新定义：

```
// The number of expected consecutive inferences for each gesture type.
// Established with the Arduino Nano 33 BLE Sense.
const int kConsecutiveInferenceThresholds[3] = {8, 5, 4};
```

如本章前文所述，该常量存储了被视为检测到每个手势所需的连续正推理次数。该次数取决于每秒执行的推理次数，因设备而异。默认值是为 SparkFun Edge 调整的，因此 Arduino 实现需要自己的一组数值。通过更改这些阈值，可以使推理更难或更容易触发，但设置过低会导致误检测。

### 在 Arduino 上获取加速度传感器数据

Arduino 加速度传感器处理器位于 [`arduino/accelerometer_handler.cc`](https://oreil.ly/jV_Qm)。它从加速度传感器获取数据并写入模型的输入缓冲区。

本次使用的模型使用 SparkFun Edge 板的数据进行训练。Edge 的加速度传感器以 25Hz 的频率提供数据，即每秒 25 次读取。为了正确运行，需要提供以相同速率捕获的数据。最终，Arduino Nano 33 BLE Sense 板的加速度传感器以 119Hz 的速率返回测量值。也就是说，不仅需要捕获数据，还需要根据模型进行 *下采样*。

听起来很专业，但下采样其实很简单。要降低信号的采样率，只需丢弃部分数据即可。让我们在下面的代码中看看其工作原理。

首先，实现包含其自身的头文件和一些其他文件：

```
#include "tensorflow/lite/micro/examples/magic_wand/
  accelerometer_handler.h"

#include <Arduino.h>
#include <Arduino_LSM9DS1.h>

#include "tensorflow/lite/micro/examples/magic_wand/constants.h"
```

`Arduino.h` 文件提供对 Arduino 平台基本功能的访问。`Arduino_LSM9DS1.h` 是 [Arduino_LSM9DS1](https://oreil.ly/eb3Zs) 库的一部分，用于与板载加速度传感器通信。

接下来，设置一些变量：

```
// A buffer holding the last 200 sets of 3-channel values
float save_data[600] = {0.0};
// Most recent position in the save_data buffer
int begin_index = 0;
// True if there is not yet enough data to run inference
bool pending_initial_data = true;
// How often we should save a measurement during downsampling
int sample_every_n;
// The number of measurements since we last saved one
int sample_skip_counter = 1;
```

这些变量包括用于存放数据的缓冲区 `save_data`、缓冲区中的当前位置以及跟踪是否有足够数据开始推理的变量。最有趣的两个变量 `sample_every_n` 和 `sample_skip_counter` 用于下采样过程。让我们再详细看看这一点。

文件接下来，`SetupAccelerometer()` 函数由程序的主循环调用，将板准备好进行数据捕获：

```
TfLiteStatus SetupAccelerometer(tflite::ErrorReporter* error_reporter) {
  // Wait until we know the serial port is ready
  while (!Serial) {
  }

  // Switch on the IMU
  if (!IMU.begin()) {
    error_reporter->Report("Failed to initialize IMU");
    return kTfLiteError;
  }
```

为了输出一切准备就绪的消息，首先要做的是确认设备的串行端口已准备就绪。然后打开 *惯性测量单元*（IMU）。IMU 是包含加速度传感器的电子组件。`IMU` 对象来自 `Arduino_LSM9DS1` 库。

下一步是开始考虑下采样。首先向 IMU 库发出查询以确定板的采样率。确定该数值后，将其除以 `kTargetHz`，该值作为目标采样率在 [`constants.h`](https://oreil.ly/rQaSw) 中定义：
