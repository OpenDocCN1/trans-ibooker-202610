
# 测试

## 模型测试

在我们的 "hello world " 应用程序中，模型只分配了 2 * 1,024 字节。`tensor_arena` 只分配了这么多字节。我们的语音模型更大、更复杂，处理输入输出，因此需要更多的空间（10 1,024）。这是通过试错决定的。

接下来，检查输入张量的大小。但是这次有点不同：

// Get information about the memory area to use for the model's input.
TfLiteTensor* input = interpreter.input(0);
// Make sure the input has the properties we expect.
TF_LITE_MICRO_EXPECT_NE(nullptr, input);
TF_LITE_MICRO_EXPECT_EQ(4, input->dims->size);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(49, input->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(40, input->dims->data[2]);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[3]);
TF_LITE_MICRO_EXPECT_EQ(kTfLiteUInt8, input->type);

因为输入处理的是频谱图，所以输入张量总共有 4 个维度。第一个维度只是一个包含一个元素的包装器。第二和第三个维度表示频谱图的“行”和“列”。输入张量最内层的第四个维度大小为 1，用于保存频谱图的各个“像素”。稍后将详细介绍频谱图的结构。

接下来，获取存储在常量 `g_yes_micro_f2e59fea_nohash_1_data` 中的“yes”频谱图样本。该常量在 [`micro_features/yes_micro_features_data.cc`](https://oreil.ly/rVn8O) 文件中定义。频谱图以一维数组形式存在，只需迭代它并将其复制到输入张量中：

// Copy a spectrogram created from a .wav audio file of someone saying "Yes"
// into the memory area used for the input.
const uint8_t* yes_features_data = g_yes_micro_f2e59fea_nohash_1_data;
for (int i = 0; i < input->bytes; ++i) {
  input->data.uint8[i] = yes_features_data[i];
}

输入赋值后，执行推理并检查输出张量的大小和形状：

// Run the model on this input and make sure it succeeds.
TfLiteStatus invoke_status = interpreter.Invoke();
if (invoke_status != kTfLiteOk) {
  error_reporter->Report("Invoke failed\n");
}
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);

// Get the output from the model, and make sure it's the expected size and
// type.
TfLiteTensor* output = interpreter.output(0);
TF_LITE_MICRO_EXPECT_EQ(2, output->dims->size);
TF_LITE_MICRO_EXPECT_EQ(1, output->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(4, output->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(kTfLiteUInt8, output->type);

我们的输出有两个维度。第一个只是包装器。另一个有 4 个元素。这是一个结构，用于保存 4 个类别（沉默、未知、“yes”、“no”）各自匹配的概率。

下一段代码块检查概率是否符合预期。由于输出张量的给定元素始终代表特定类别，因此知道应检查每个类别的哪个索引。顺序在训练期间定义：

// There are four possible classes in the output, each with a score.
const int kSilenceIndex = 0;
const int kUnknownIndex = 1;
const int kYesIndex = 2;
const int kNoIndex = 3;

// Make sure that the expected "Yes" score is higher than the other classes.
uint8_t silence_score = output->data.uint8[kSilenceIndex];
uint8_t unknown_score = output->data.uint8[kUnknownIndex];
uint8_t yes_score = output->data.uint8[kYesIndex];
uint8_t no_score = output->data.uint8[kNoIndex];
TF_LITE_MICRO_EXPECT_GT(yes_score, silence_score);
TF_LITE_MICRO_EXPECT_GT(yes_score, unknown_score);
TF_LITE_MICRO_EXPECT_GT(yes_score, no_score);

由于传递了“是”的频谱图，因此预计变量 `yes_score` 的概率高于 `silence_score`、`unknown_score`、`no_score`。

对“是”满意后，用“否”的频谱图做同样的事情。首先复制输入并执行推理：

// Now test with a different input, from a recording of "No".
const uint8_t* no_features_data = g_no_micro_f9643d42_nohash_4_data;
for (int i = 0; i < input->bytes; ++i) {
  input->data.uint8[i] = no_features_data[i];
}
// Run the model on this "No" input.
invoke_status = interpreter.Invoke();
if (invoke_status != kTfLiteOk) {
  error_reporter->Report("Invoke failed\n");
}
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);

推理结束后，确认“不”获得了最高分数：

// Make sure that the expected "No" score is higher than the other classes.
silence_score = output->data.uint8[kSilenceIndex];
unknown_score = output->data.uint8[kUnknownIndex];
yes_score = output->data.uint8[kYesIndex];
no_score = output->data.uint8[kNoIndex];
TF_LITE_MICRO_EXPECT_GT(no_score, silence_score);
TF_LITE_MICRO_EXPECT_GT(no_score, unknown_score);
TF_LITE_MICRO_EXPECT_GT(no_score, yes_score);

就这样结束了！

要运行此测试，请从 TensorFlow 仓库的根目录执行以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_micro_speech_test
```

接下来，让我们看看音频提供程序，它是所有音频数据的来源。

## 音频提供程序

音频提供程序将设备的麦克风硬件连接到我们的代码。所有设备都有不同的音频捕获机制。因此，[`audio_provider.h`是](https://oreil.ly/89FGG)定义了请求音频数据的接口，开发者可以根据想要支持的平台编写自己的实现。

### 芯片

此示例包含 Arduino、STM32F746G、SparkFun Edge、macOS 的音频提供程序实现。如果您想在此示例中支持新设备，可以阅读现有实现并学习其方法。

音频提供程序的核心部分是 `audio_provider.h` 中定义的 `GetAudioSamples()` 函数。它如下所示：

```
TfLiteStatus GetAudioSamples(tflite::ErrorReporter* error_reporter,
                             int start_ms, int duration_ms,
                             int* audio_samples_size, int16_t** audio_samples);
```

如 `audio_provider.h` 中所述，此函数预计返回 16 位脉冲编码调制（PCM）音频数据的数组。这是数字音频中非常常见的格式。

此函数指定 `ErrorReporter` 实例、开始时间 (`start_ms`)、持续时间 (`duration_ms`)、两个指针进行调用。

这些指针是 `GetAudioSamples()` 提供数据的机制。调用方声明适当类型的变量，并在函数调用时传递该变量的指针。在函数实现内部，指针被解引用并设置变量的值。

第一个指针 `audio_samples_size` 中包含音频数据的 16 位样本总数。第二个指针 `audio_samples` 接收包含音频数据本身的数组。

通过查看测试可以实际看到这一点。[`audio_provider_test.cc`中有](https://oreil.ly/9XgFg)两个测试，但要学习音频提供程序的使用方法，只需查看第一个测试即可：

```
TF_LITE_MICRO_TEST(TestAudioProvider) {
  tflite::MicroErrorReporter micro_error_reporter;
  tflite::ErrorReporter* error_reporter = &micro_error_reporter;

  int audio_samples_size = 0;
  int16_t* audio_samples = nullptr;
  TfLiteStatus get_status =
      GetAudioSamples(error_reporter, 0, kFeatureSliceDurationMs,
                      &audio_samples_size, &audio_samples);
  TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, get_status);
  TF_LITE_MICRO_EXPECT_LE(audio_samples_size, kMaxAudioSampleSize);
  TF_LITE_MICRO_EXPECT_NE(audio_samples, nullptr);

  // Make sure we can read all of the returned memory locations.
  int total = 0;
  for (int i = 0; i < audio_samples_size; ++i) {
    total += audio_samples[i];
  }
}
```

此测试展示了 `GetAudioSamples()` 如何使用一些值和一些指针进行调用。此测试验证函数调用后指针是否正确赋值。

### 注

你会注意到使用了常量 `kFeatureSliceDurationMs` 和 `kMaxAudioSampleSize`。这些是在模型训练时选择的值，可以在 [`micro_features/micro_model_settings.h`中](https://oreil.ly/WLuug)找到。

`audio_provider.cc` 的默认实现只是返回一个空数组。为了证明它是正确的大小，测试只是简单地循环预期样本数。

音频提供程序除了 `GetAudioSamples()` 外，还有一个 `LatestAudioTimestamp()` 函数调用。它返回音频数据最后捕获时间的毫秒数。此信息对于特征提供程序确定要获取的音频数据是必要的。

要运行音频提供程序的测试，请使用以下命令：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_audio_provider_test
```

音频提供程序被特征提供程序用作新鲜音频样本的来源。

## 特征提供程序

特征提供程序将从音频提供程序获取的原始音频转换为模型可以输入的频谱图。它在主循环中调用。

其接口在 [`feature_provider.h`中](https://oreil.ly/59uTO)定义，如下所示：

```
class FeatureProvider {
 public:
  // Create the provider, and bind it to an area of memory. This memory should
  // remain accessible for the lifetime of the provider object, since subsequent
  // calls will fill it with feature data. The provider does no memory
  // management of this data.
  FeatureProvider(int feature_size, uint8_t* feature_data);
  ~FeatureProvider();

  // Fills the feature data with information from audio inputs, and returns how
  // many feature slices were updated.
  TfLiteStatus PopulateFeatureData(tflite::ErrorReporter* error_reporter,
                                   int32_t last_time_in_ms, int32_t time_in_ms,
                                   int* how_many_new_slices);

 private:
  int feature_size_;
  uint8_t* feature_data_;
  // Make sure we don't try to use cached information if this is the first call
  // into the provider.
  bool is_first_run_;
};
```

要了解它是如何使用的，请查看 [`feature_provider_mock_test.cc`的](https://oreil.ly/N3YPu)测试。
