
## 输入张量的模拟

首先，为了模拟模型的输入张量，准备一个名为 `input` 的 `float` 数组。由于有 128 个 3 轴读数，因此总长度为 384 个（128 * 3）。将数组的所有值初始化为 `0.0`。

接下来调用 `ReadAccelerometer()`。在这里，指定 `ErrorReporter` 实例、写入数据的数组（`input`）以及想要获取的数据总量（384 字节）。最后一个参数是布尔值标志，指示在 `ReadAccelerometer()` 进一步读取数据之前是否清除缓冲区。这需要在手势被成功识别后执行。

被调用时，`ReadAccelerometer()` 函数会尝试向传递的数组写入 384 字节的数据。如果加速度传感器刚刚开始收集数据，可能 384 字节的数据尚未可用。在这种情况下，函数什么也不做，并返回 `false` 作为返回值。可以利用这一点避免在数据不可用时执行推理。

[`accelerometer_handler.cc` 中](https://oreil.ly/MwM7g) 的加速度传感器处理器的虚拟实现模拟了每次调用时都可以进行另一次读取。通过再调用 127 次，可以获得足够的数据以返回 `true`。

要运行这些测试，请使用以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_gesture_accelerometer_handler_test
```

## 手势预测

推理完成后，输出张量会被概率填充，指示执行了哪个手势。然而，机器学习并不是一门精确的科学，因此一次推理也可能出现假阳性。

为了减少假阳性的影响，可以规定手势必须在至少一定数量的连续推理中被检测到才会被识别。考虑到每秒可以执行多次推理，可以快速判断结果是否有效。这就是手势预测器的任务。

定义一个以模型输出张量作为输入的单一函数 `PredictGesture()`。为了判断是否检测到手势，该函数执行两件事：

1. 检查手势的概率是否满足最低阈值
2. 检查手势是否在一定次数的推理中持续被检测到

所需的最小推理次数因手势而异。此外，由于更快的设备可以更频繁地执行推理，因此也因设备而异。针对 SparkFun Edge 板调整的默认值在 [`constants.cc` 中](https://oreil.ly/ktGgw)：

```
const int kConsecutiveInferenceThresholds[3] = {15, 12, 10};
```

值是按照手势在模型输出张量中出现的相同顺序定义的。对于 Arduino 等其他平台，该文件有设备特定的版本，其中包含根据各自性能调整的值。

查看 [`gesture_predictor.cc` 的](https://oreil.ly/f3I6U) 代码，看看它们是如何使用的。

首先，定义变量来跟踪最后看到的手势以及同一手势连续记录了多少次：

```
// How many times the most recent gesture has been matched in a row
int continuous_count = 0;
// The result of the last prediction
int last_predict = -1;
```

接下来，定义 `PredictGesture()` 函数，判断是否有任何手势类别在最近的推理中概率超过 0.8：

```
// Return the result of the last prediction
// 0: wing("W"), 1: ring("O"), 2: slope("angle"), 3: unknown
int PredictGesture(float* output) {
  // Find whichever output has a probability > 0.8 (they sum to 1)
  int this_predict = -1;
  for (int i = 0; i < 3; i++) {
    if (output[i] > 0.8) this_predict = i;
  }
```

使用 `this_predict` 保存预测的手势的索引。

变量 `continuous_count` 用于跟踪最近检测到的手势连续被预测了多少次。如果没有任何手势类别满足概率阈值 0.8，则重置正在进行的检测过程。通过将 `continuous_count` 设置为 `0`，将 `last_predict` 设置为 `3`（“未知”类别的索引），重置正在进行的检测过程，并表明最近的结果不是已知的手势：

```
  // No gesture was detected above the threshold
  if (this_predict == -1) {
    continuous_count = 0;
    last_predict = 3;
    return 3;
  }
```

接下来，如果最近的预测与之前的预测一致，则递增 `continuous_count`。否则，将其重置为 `0`。同时，将最新预测存储到 `last_predict` 中：

```
  if (last_predict == this_predict) {
    continuous_count += 1;
  } else {
    continuous_count = 0;
  }
  last_predict = this_predict;
```

`PredictGesture()` 的下一部分使用 `continuous_count` 检查当前手势是否仍满足阈值。如果不满足，则返回表示未知手势的 `3`：

```
  // If we haven't yet had enough consecutive matches for this gesture,
  // report a negative result
  if (continuous_count < kConsecutiveInferenceThresholds[this_predict]) {
    return 3;
  }
```

超过这一点，就意味着确认了一个有效手势。在这种情况下，重置所有变量：

```
  // Otherwise, we've seen a positive result, so clear all our variables
  // and report it
  continuous_count = 0;
  last_predict = -1;
  return this_predict;
}
```

函数通过返回当前预测值结束。它从主循环传递给输出处理器，输出处理器将结果显示给用户。

手势预测器的测试在 [`gesture_predictor_test.cc` 中](https://oreil.ly/5BZzt)。第一个测试展示了预测的成功：

```
TF_LITE_MICRO_TEST(SuccessfulPrediction) {
  // Use the threshold from the 0th gesture
  int threshold = kConsecutiveInferenceThresholds[0];
  float probabilities[4] = {1.0, 0.0, 0.0, 0.0};
  int prediction;
  // Loop just too few times to trigger a prediction
  for (int i = 0; i <= threshold - 1; i++) {
    prediction = PredictGesture(probabilities);
    TF_LITE_MICRO_EXPECT_EQ(prediction, 3);
  }
  // Call once more, triggering a prediction
  // for category 0
  prediction = PredictGesture(probabilities);
  TF_LITE_MICRO_EXPECT_EQ(prediction, 0);
}
```

向 `PredictGesture()` 函数输入一组强烈表明第一个类别匹配的概率。然而，在以这些概率调用 `threshold` 次之前，它会返回表示“未知”的结果 `3`。当调用 `threshold` 次时，会返回针对类别 `0` 的正预测。

下一个测试展示了当某个类别的高概率连续执行被另一个类别的高概率中断时会发生什么：

```
TF_LITE_MICRO_TEST(FailPartWayThere) {
  // Use the threshold from the 0th gesture
  int threshold = kConsecutiveInferenceThresholds[0];
  float probabilities[4] = {1.0, 0.0, 0.0, 0.0};
  int prediction;
  // Loop just too few times to trigger a prediction
  for (int i = 0; i <= threshold - 1; i++) {
    prediction = PredictGesture(probabilities);
    TF_LITE_MICRO_EXPECT_EQ(prediction, 3);
  }
  // Call with a different prediction, triggering a failure
  probabilities[0] = 0.0;
  probabilities[2] = 1.0;
  prediction = PredictGesture(probabilities);
  TF_LITE_MICRO_EXPECT_EQ(prediction, 3);
}
```

在这种情况下，输入一组类别 `0` 的连续高概率，但数量不足以满足阈值。接下来，修改概率使类别 `2` 最高。结果预测类别 `3`，表示“未知”手势。

最后的测试展示了 `PredictGesture()` 如何忽略低于阈值的概率。在循环中，输入恰好满足 `0` 的阈值的预测次数。然而，类别 `0` 的概率最高，但其值为 0.7，低于 `PredictGesture()` 内部阈值 0.8。结果为类别 `3` “未知”预测：

```
TF_LITE_MICRO_TEST(InsufficientProbability) {
  // Use the threshold from the 0th gesture
  int threshold = kConsecutiveInferenceThresholds[0];
  // Below the probability threshold of 0.8
  float probabilities[4] = {0.7, 0.0, 0.0, 0.0};
  int prediction;
  // Loop the exact right number of times
  for (int i = 0; i <= threshold; i++) {
    prediction = PredictGesture(probabilities);
    TF_LITE_MICRO_EXPECT_EQ(prediction, 3);
  }
}
```

要运行这些测试，请使用以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_gesture_predictor_test
```

## 输出处理器

输出处理器非常简单，只接收 `PredictGesture()` 返回的类索引并将结果显示给用户。[`output_handler_test.cc` 中](https://oreil.ly/QWkeL) 的测试展示了其接口：

```
TF_LITE_MICRO_TEST(TestCallability) {
  tflite::MicroErrorReporter micro_error_reporter;
  tflite::ErrorReporter* error_reporter = &micro_error_reporter;
  HandleOutput(error_reporter, 0);
  HandleOutput(error_reporter, 1);
  HandleOutput(error_reporter, 2);
  HandleOutput(error_reporter, 3);
}
```

要运行此测试，请使用以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_gesture_output_handler_test
```

## 检测手势

所有这些组件都 [`main_functions.cc` 中汇总](https://oreil.ly/ggNtD)，其中包含程序的核心逻辑。首先，设置常规变量和一些额外变量：

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

// Globals, used for compatibility with Arduino-style sketches.
namespace {
tflite::ErrorReporter* error_reporter = nullptr;
const tflite::Model* model = nullptr;
tflite::MicroInterpreter* interpreter = nullptr;
TfLiteTensor* model_input = nullptr;
int input_length;

// Create an area of memory to use for input, output, and intermediate arrays.
// The size of this will depend on the model you're using, and may need to be
// determined by experimentation.
constexpr int kTensorArenaSize = 60 * 1024;
uint8_t tensor_arena[kTensorArenaSize];

// Whether we should clear the buffer next time we fetch data
bool should_clear_buffer = false;
}  // namespace
```

变量 `input_length` 存储模型的输入张量的长度，变量 `should_clear_buffer` 是指示下次执行时是否应清除加速度传感器处理器的缓冲区的标志。缓冲区的清除在检测成功后进行，为后续推理提供干净的状态。

接下来，`setup()` 函数执行所有常规的 housekeeping 操作，以便准备好执行推理：
