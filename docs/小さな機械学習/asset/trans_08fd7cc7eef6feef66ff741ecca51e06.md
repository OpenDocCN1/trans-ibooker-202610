
### [`加速度センサー・ハンドラー・テスト.cc`](https://oreil.ly/MwM7g)

使用加速度传感器处理器获取最新数据的方法。

### [`gesture_predictor_test.cc`](https://oreil.ly/cGbim)

说明如何使用手势预测器来解释推理结果。

### [`output_handler_test.cc`](https://oreil.ly/MYwUW)

说明输出处理器用于显示推理结果的用法。

首先，让我们来看一下 `magic_wand_test.cc`。它展示了我们模型推理的端到端过程。

## 基本流程

在 `magic_wand_test.cc` 中说明基本流程。

首先，列出模型所需的算子：

```
namespace tflite {
namespace ops {
namespace micro {
TfLiteRegistration* Register_DEPTHWISE_CONV_2D();
TfLiteRegistration* Register_MAX_POOL_2D();
TfLiteRegistration* Register_CONV_2D();
TfLiteRegistration* Register_FULLY_CONNECTED();
TfLiteRegistration* Register_SOFTMAX();
}  // namespace micro
}  // namespace ops
}  // namespace tflite
```

测试本身（像往常一样）从设置推理所需的一切并获取指向模型输入张量的指针开始：

// Set up logging
tflite::MicroErrorReporter micro_error_reporter;
tflite::ErrorReporter* error_reporter = &micro_error_reporter;

// Map the model into a usable data structure. This doesn't involve any
// copying or parsing, it's a very lightweight operation.
const tflite::Model* model =
    ::tflite::GetModel(g_magic_wand_model_data);
if (model->version() != TFLITE_SCHEMA_VERSION) {
error_reporter->Report(
    "Model provided is schema version %d not equal "
    "to supported version %d.\n",
    model->version(), TFLITE_SCHEMA_VERSION);
}

static tflite::MicroMutableOpResolver micro_mutable_op_resolver;
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

// Create an area of memory to use for input, output, and intermediate arrays.
// Finding the minimum value for your model may require some trial and error.
const int tensor_arena_size = 60 * 1024;
uint8_t tensor_arena[tensor_arena_size];

// Build an interpreter to run the model with
tflite::MicroInterpreter interpreter(model, micro_mutable_op_resolver, tensor_arena,
                                    tensor_arena_size, error_reporter);

// Allocate memory from the tensor_arena for the model's tensors
interpreter.AllocateTensors();

// Obtain a pointer to the model's input tensor
TfLiteTensor* input = interpreter.input(0);
```

接下来，检查输入张量并确认其具有预期的形状：

// Make sure the input has the properties we expect
TF_LITE_MICRO_EXPECT_NE(nullptr, input);
TF_LITE_MICRO_EXPECT_EQ(4, input->dims->size);
// The value of each element gives the length of the corresponding tensor.
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(128, input->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(3, input->dims->data[2]);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[3]);
// The input is a 32 bit floating point value
TF_LITE_MICRO_EXPECT_EQ(kTfLiteFloat32, input->type);
```

输入形状为 `(1, 128, 3, 1)`。第一个维度只是第二个维度的包装器，后者保存 128 个三轴加速度传感器的读数。每个读数有 3 个元素，每个轴一个，每个值都被包装在一个单元素张量中。输入全部为 32 位浮点值。

确认输入形状后，将数据写入输入张量：

// Provide an input value
const float* ring_features_data = g_circle_micro_f9643d42_nohash_4_data;
error_reporter->Report("%d", input->bytes);
for (int i = 0; i < (input->bytes / sizeof(float)); ++i) {
    input->data.f[i] = ring_features_data[i];
}
```

常量 `g_circle_micro_f9643d42_nohash_4_data` 在 `circle_micro_features_data.cc` 中定义。它作为容器存储表示一个人尝试画圆手势的浮点值数组。`for` 循环遍历此数据并将每个值写入输入。写入与输入张量可容纳的数量相同数量的 `float` 值。

接下来，以熟悉的方式运行推理：

// Run the model on this input and check that it succeeds
TfLiteStatus invoke_status = interpreter.Invoke();
if (invoke_status != kTfLiteOk) {
  error_reporter->Report("Invoke failed\n");
}
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);
```

然后，检查输出张量并确认其具有预期的形状：

// Obtain a pointer to the output tensor and make sure it has the
// properties we expect.
TfLiteTensor* output = interpreter.output(0);
TF_LITE_MICRO_EXPECT_EQ(2, output->dims->size);
TF_LITE_MICRO_EXPECT_EQ(1, output->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(4, output->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(kTfLiteFloat32, output->type);
```

它是一个单元素包装器和一组 4 个值，表示 4 个概率（"wing"、"ring"、"slope"、"unknown"）。它们各自为 32 位浮点数。

接下来，可以测试数据以确认推理结果符合预期。由于传递了圆手势的数据，因此预计“ring”得分最高：

// There are four possible classes in the output, each with a score.
const int kWingIndex = 0;
const int kRingIndex = 1;
const int kSlopeIndex = 2;
const int kNegativeIndex = 3;

// Make sure that the expected "Ring" score is higher than the other
// classes.
float wing_score = output->data.f[kWingIndex];
float ring_score = output->data.f[kRingIndex];
float slope_score = output->data.f[kSlopeIndex];
float negative_score = output->data.f[kNegativeIndex];
TF_LITE_MICRO_EXPECT_GT(ring_score, wing_score);
TF_LITE_MICRO_EXPECT_GT(ring_score, slope_score);
TF_LITE_MICRO_EXPECT_GT(ring_score, negative_score);
```

然后，用“slope”手势重复这一系列过程：

```
  // Now test with a different input, from a recording of "Slope".
  const float* slope_features_data = g_angle_micro_f2e59fea_nohash_1_data;
  for (int i = 0; i < (input->bytes / sizeof(float)); ++i) {
    input->data.f[i] = slope_features_data[i];
  }

  // Run the model on this "Slope" input.
  invoke_status = interpreter.Invoke();
  if (invoke_status != kTfLiteOk) {
    error_reporter->Report("Invoke failed\n");
  }
  TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);

  // Make sure that the expected "Slope" score is higher than the other classes.
  wing_score = output->data.f[kWingIndex];
  ring_score = output->data.f[kRingIndex];
  slope_score = output->data.f[kSlopeIndex];
  negative_score = output->data.f[kNegativeIndex];
  TF_LITE_MICRO_EXPECT_GT(slope_score, wing_score);
  TF_LITE_MICRO_EXPECT_GT(slope_score, ring_score);
  TF_LITE_MICRO_EXPECT_GT(slope_score, negative_score);
```

就这样！我们看到了如何从原始加速度传感器数据执行推理。与之前的示例一样，能够避免预处理意味着可以保持事物的简单性。

要运行此测试，请使用以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile test_magic_wand_test
```

## 加速度传感器处理器

下一个测试展示了加速度传感器处理器的接口。该组件的任务是将每次推理的加速度计数据输入到输入张量中。

由于这些都取决于设备的加速度传感器的工作方式，因此为每个设备提供了不同的加速度传感器处理器实现。稍后将介绍这些实现，但现在 [`accelerometer_handler_test.cc`](https://oreil.ly/MwM7g) 中的测试将展示处理器应如何被调用。

第一个测试非常简单：

```
TF_LITE_MICRO_TEST(TestSetup) {
  static tflite::MicroErrorReporter micro_error_reporter;
  TfLiteStatus setup_status = SetupAccelerometer(&micro_error_reporter);
  TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, setup_status);
}
```

`SetupAccelerometer()` 函数执行获取加速度传感器值所需的一次性设置。此测试展示了函数如何被调用（带有指向 `ErrorReporter` 的指针）并返回指示设置成功的 `TfLiteStatus`。

下一个测试展示了加速度传感器处理器如何用于用数据填充输入张量：

```
TF_LITE_MICRO_TEST(TestAccelerometer) {
  float input[384] = {0.0};
  tflite::MicroErrorReporter micro_error_reporter;
  // Test that the function returns false before insufficient data is available
  bool inference_flag =
      ReadAccelerometer(&micro_error_reporter, input, 384, false);
  TF_LITE_MICRO_EXPECT_EQ(inference_flag, false);

  // Test that the function returns true once sufficient data is available to
  // fill the model's input buffer (128 sets of values)
  for (int i = 1; i <= 128; i++) {
    inference_flag =
        ReadAccelerometer(&micro_error_reporter, input, 384, false);
  }
  TF_LITE_MICRO_EXPECT_EQ(inference_flag, true);
}
```
