
拼图的最后一块——命令响应器会生成通知单词已被检测到的输出。

命令响应器被设计为可以按设备类型进行覆盖。有关设备特定的实现，本章后半部分会进行说明。

先来看看一个非常简单的参考实现，它只是将检测结果作为文本记录到日志中。可以在 [`command_responder.cc`](https://oreil.ly/kMjg2) 中找到：

```
void RespondToCommand(tflite::ErrorReporter* error_reporter,
                      int32_t current_time, const char* found_command,
                      uint8_t score, bool is_new_command) {
  if (is_new_command) {
    error_reporter->Report("Heard %s (%d) @%dms", found_command, score,
                           current_time);
  }
}
```

就这么多！该文件实现的函数只有一个，即 `RespondToCommand()`。其参数包括 `error_reporter`、当前时间（`current_time`）、最后检测到的命令（`found_command`）、该命令获得的分数（`score`），以及该命令是否新出现（`is_new_command`）。

在此程序的主循环中，需要注意的是，即使未检测到命令，每次执行推理时都会调用此函数。也就是说，需要检查 `is_new_command` 来判断是否需要做些什么。

位于 [`command_responder_test.cc`](https://oreil.ly/loLZo) 的该函数测试同样简单。由于无法测试是否生成了正确的输出，因此只是调用该函数：

```
TF_LITE_MICRO_TEST(TestCallability) {
  tflite::MicroErrorReporter micro_error_reporter;
  tflite::ErrorReporter* error_reporter = &micro_error_reporter;

  // This will have external side-effects (like printing to the debug console
  // or lighting an LED) that are hard to observe, so the most we can do is
  // make sure the call doesn't crash.
  RespondToCommand(error_reporter, 0, "foo", 0, true);
}
```

要运行此测试，请在终端中输入以下内容：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_command_responder_test
```

就到这里！我们已经看到了应用程序的所有组成部分。现在让我们看看它们在编程中是如何组合起来的。

# 倾听唤醒词

下面的代码可以在 [`main_functions.cc`](https://oreil.ly/n2eD1) 中找到。它定义了本次编程的核心 `setup()` 和 `loop()` 函数。让我们一起阅读！

你已经是 TensorFlow Lite 的熟练使用者了，所以这段代码的许多部分看起来应该很熟悉。那么让我们聚焦于新内容。

首先，列出想要使用的操作：

```
namespace tflite {
namespace ops {
namespace micro {
TfLiteRegistration* Register_DEPTHWISE_CONV_2D();
TfLiteRegistration* Register_FULLY_CONNECTED();
TfLiteRegistration* Register_SOFTMAX();
}  // namespace micro
}  // namespace ops
}  // namespace tflite
```

接下来，设置全局变量：

```
namespace {
tflite::ErrorReporter* error_reporter = nullptr;
const tflite::Model* model = nullptr;
tflite::MicroInterpreter* interpreter = nullptr;
TfLiteTensor* model_input = nullptr;
FeatureProvider* feature_provider = nullptr;
RecognizeCommands* recognizer = nullptr;
int32_t previous_time = 0;

// Create an area of memory to use for input, output, and intermediate arrays.
// The size of this will depend on the model you're using, and may need to be
// determined by experimentation.
constexpr int kTensorArenaSize = 10 * 1024;
uint8_t tensor_arena[kTensorArenaSize];
}  // namespace
```

除了通常的 TensorFlow 嫌疑对象外，请注意还声明了 `FeatureProvider` 和 `RecognizeCommands`。此外，还声明了名为 `g_previous_time` 的变量。它记录了最近接收到新音频样本的时间。

接下来，在 `setup()` 函数中，加载模型、设置解释器、添加操作并分配张量：

```
void setup() {
  // Set up logging.
  static tflite::MicroErrorReporter micro_error_reporter;
  error_reporter = &micro_error_reporter;

  // Map the model into a usable data structure. This doesn't involve any
  // copying or parsing, it's a very lightweight operation.
  model = tflite::GetModel(g_tiny_conv_micro_features_model_data);
  if (model->version() != TFLITE_SCHEMA_VERSION) {
    error_reporter->Report(
        "Model provided is schema version %d not equal "
        "to supported version %d.",
        model->version(), TFLITE_SCHEMA_VERSION);
    return;
  }

  // Pull in only the operation implementations we need.
  static tflite::MicroMutableOpResolver micro_mutable_op_resolver;
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_DEPTHWISE_CONV_2D,
      tflite::ops::micro::Register_DEPTHWISE_CONV_2D());
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_FULLY_CONNECTED,
      tflite::ops::micro::Register_FULLY_CONNECTED());
  micro_mutable_op_resolver.AddBuiltin(tflite::BuiltinOperator_SOFTMAX,
                                       tflite::ops::micro::Register_SOFTMAX());

  // Build an interpreter to run the model with.
  static tflite::MicroInterpreter static_interpreter(
      model, micro_mutable_op_resolver, tensor_arena, kTensorArenaSize,
      error_reporter);
  interpreter = &static_interpreter;

  // Allocate memory from the tensor_arena for the model's tensors.
  TfLiteStatus allocate_status = interpreter->AllocateTensors();
  if (allocate_status != kTfLiteOk) {
    error_reporter->Report("AllocateTensors() failed");
    return;
  }
```

在分配张量之后，检查输入张量的形状和类型是否正确：

```
  // Get information about the memory area to use for the model's input.
  model_input = interpreter->input(0);
  if ((model_input->dims->size != 4) || (model_input->dims->data[0] != 1) ||
      (model_input->dims->data[1] != kFeatureSliceCount) ||
      (model_input->dims->data[2] != kFeatureSliceSize) ||
      (model_input->type != kTfLiteUInt8)) {
    error_reporter->Report("Bad input tensor parameters in model");
    return;
  }
```

接下来有趣的事情开始了。首先实例化 `FeatureProvider`，并将其指向输入张量：

```
  // Prepare to access the audio spectrograms from a microphone or other source
  // that will provide the inputs to the neural network.
  static FeatureProvider static_feature_provider(kFeatureElementCount,
                                                 model_input->data.uint8);
  feature_provider = &static_feature_provider;
```

接下来，创建 `RecognizeCommands` 实例并初始化 `previous_time` 变量：

```
  static RecognizeCommands static_recognizer(error_reporter);
  recognizer = &static_recognizer;

  previous_time = 0;
}
```

接下来是 `loop()` 函数。与之前的示例一样，此函数会被无限次反复调用。在循环中，首先使用特征提供程序创建频谱图：

```
void loop() {
  // Fetch the spectrogram for the current time.
  const int32_t current_time = LatestAudioTimestamp();
  int how_many_new_slices = 0;
  TfLiteStatus feature_status = feature_provider->PopulateFeatureData(
      error_reporter, previous_time, current_time, &how_many_new_slices);
  if (feature_status != kTfLiteOk) {
    error_reporter->Report("Feature generation failed");
    return;
  }
  previous_time = current_time;
  // If no new audio samples have been received since last time, don't bother
  // running the network model.
  if (how_many_new_slices == 0) {
    return;
  }
```

如果从上一次迭代以来没有新数据，则无需执行推理。

输入准备好后，只需调用解释器即可：

```
  // Run the model on the spectrogram input and make sure it succeeds.
  TfLiteStatus invoke_status = interpreter->Invoke();
  if (invoke_status != kTfLiteOk) {
    error_reporter->Report("Invoke failed");
    return;
  }
```

模型的输出张量被每个类别的概率填充。为了解释它，使用 `RecognizeCommands` 实例。获取指向输出张量的指针，并设置一些变量以接收 `ProcessLatestResults()` 的输出：

```
  // Obtain a pointer to the output tensor
  TfLiteTensor* output = interpreter->output(0);
  // Determine whether a command was recognized based on the output of inference
  const char* found_command = nullptr;
  uint8_t score = 0;
  bool is_new_command = false;
  TfLiteStatus process_status = recognizer->ProcessLatestResults(
      output, current_time, &found_command, &score, &is_new_command);
  if (process_status != kTfLiteOk) {
    error_reporter->Report("RecognizeCommands::ProcessLatestResults() failed");
    return;
  }
```

最后，为了在检测到单词时通知用户，调用命令响应器的 `RespondToCommand()` 方法：

```
  // Do something based on the recognized command. The default implementation
  // just prints to the error console, but you should replace this with your
  // own function for a real application.
  RespondToCommand(error_reporter, current_time, found_command, score,
                   is_new_command);
}
```

就到这里了！对 `RespondToCommand()` 的调用是循环的最后一步。从特征生成开始的所有处理将不断重复检查音频中是否包含已知单词，并在确认单词后发出某种输出。

`setup()` 和 `loop()` 函数由在 `main.cc` 中定义的 `main()` 函数调用。该函数在应用程序启动时开始循环：
