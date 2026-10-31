
与之前的章节不同，运行此应用程序需要额外添加一些硬件。由于两个板上都没有内置摄像头，因此建议购买摄像头模块。此信息可在各设备的部分中找到。

先从应用程序的结构开始看起。比想象中简单得多。

# 应用程序架构

到目前为止，我们了解到嵌入式机器学习应用程序执行以下序列：

1. 获取输入。
2. 为了提取适合作为模型输入的特征，对输入进行预处理。
3. 对处理后的输入执行推理。
4. 对模型的输出进行后处理使其具有意义。
5. 利用由此获得的信息使事情发生。

在[第7章](#ch07.html#chapter_speech_wake_word_example)中，我们将其应用于以语音为输入的唤醒词检测。这次输入的是图像数据。听起来可能更复杂，但实际上比语音简单得多。

图像数据通常表示为像素值的数组。我们从嵌入式摄像头模块获取图像数据，它们都以这种格式提供数据。我们的模型也假设输入是像素值数组。因此，在将数据输入模型之前几乎不需要预处理。

考虑到几乎不需要预处理，我们的应用程序会相当简单。拍摄来自摄像头的数据的快照，将其输入模型，判断检测到哪个输出类别。然后以简单的方式显示结果。

在继续之前，让我们先多了解一下本次使用的模型。

## 模型介绍

在[第7章](#ch07.html#chapter_speech_wake_word_example)中，我们了解到卷积神经网络是一种设计用于很好处理多维张量的神经网络，其中信息包含在相邻值组之间的关系中。特别适合处理图像数据。

我们的目标人物检测模型是一个在[Visual Wake Words 数据集上](https://oreil.ly/EC6nd)训练的卷积神经网络。该数据集由115,000张图像组成，每张图像都标注了是否包含人物。

该模型为250KB，比我们的语音模型大得多。不仅占用更多内存，这个额外大小还意味着执行一次推理需要花费大量时间。

该模型接受96×96像素的灰度图像作为输入。每张图像作为形状为`(96, 96, 1)`的3维张量提供，最后一个维度包含表示一个像素的8位值。该值指定像素的色调，范围从0（完全黑色）到255（完全白色）。

摄像头模块返回各种分辨率的图像，因此需要将其调整为96×96像素。此外，还需要将全彩图像转换为灰度，以便模型使用。

96×96像素可能看起来分辨率很小，但对于从每张图像中检测人物来说已经绰绰有余。处理图像的模型通常接受惊人小的分辨率。随着模型输入尺寸的增大，收益递减，而输入尺寸越大，网络的复杂性就会大幅增加。因此，即使是最先进的图像分类模型，通常也最多在320×320像素下运行。

一个表示输入中有人存在的概率，另一个表示无人存在的概率。概率范围为0到255。

我们的目标人物检测模型使用`MobileNet`架构。该架构是一种设计用于在智能手机等设备上进行图像分类的知名且经过验证的架构。在[第10章](#ch10.html#chapter_person_detection_training)中，我们将学习该模型是如何适配到微控制器的，以及如何训练自己的模型。暂时，让我们继续探索我们的应用程序如何工作。

## 所有可动部件

[图9-](#ch09.html#application_architecture_2)1显示了目标人物检测应用程序的结构。

![](assets/timl_0901.png)

正如前面所述，由于可以将图像数据直接传递给模型，因此不需要预处理。

保持事情简单的一个方面是，不对模型的输出进行平均化。我们的唤醒词模型每秒执行多次，因此为了获得稳定的结果需要对输出进行平均化。人物检测模型更大，执行推理需要时间。因此，不需要对输出进行平均化。

代码有五个主要部分：

主循环

: 

与其它示例类似，此应用程序在连续循环中运行。然而，我们的模型更大更复杂，因此执行推理需要时间。根据设备，预期的是每几秒一次推理，而不是每秒几次。

图像提供者

: 

此组件从摄像头捕获图像数据并将其写入输入张量。由于捕获图像的方法因设备而异，因此可以覆盖此组件进行自定义。

TensorFlow Lite 解释器

: 

解释器运行 TensorFlow Lite 模型，将输入图像转换为概率集合。

模型

: 

模型作为数据数组包含在内，由解释器执行。该模型为250KB，对于 TensorFlow GitHub 仓库来说太大而无法提交。因此，在项目构建时由`Makefile`下载。如果想查看，可以从[`tf_lite_micro_person_data_grayscale.zip`从](https://oreil.ly/Ylq9m)下载。

检测响应器

: 

检测响应器接收模型输出的概率，并使用设备的输出功能将其显示出来。此响应器可以根据设备类型进行覆盖。此代码示例中点亮 LED，但也可以扩展以实现几乎任何功能。

为了了解这些部件如何组合起来，让我们看看各自的测试。

# 通过测试逐步了解

此应用程序简单且良好。所有内容都可以在[GitHub 仓库中](https://oreil.ly/31vB5)找到：

[`人物检测测试.cc`](https://oreil.ly/r4ny8)

: 

展示如何对表示一张图像的数组执行推理。

[`image_provider_test.cc`](https://oreil.ly/Js6M3)

: 

展示如何使用图像提供者捕获图像

[`detection_responder_test.cc`](https://oreil.ly/KBVLF)

: 

展示如何使用检测响应器输出检测结果。

首先，检查`person_detection_test.cc`，看看如何对图像数据执行推理。这是第三个示例，所以这段代码应该感觉很熟悉。你正在顺利地走向嵌入式 ML 开发者的道路！

## 基本流程

首先是`person_detection_test.cc`。首先从导入模型所需的运算符开始：

```
namespace tflite {
namespace ops {
namespace micro {
TfLiteRegistration* Register_DEPTHWISE_CONV_2D();
TfLiteRegistration* Register_CONV_2D();
TfLiteRegistration* Register_AVERAGE_POOL_2D();
}  // namespace micro
}  // namespace ops
}  // namespace tflite
```

接下来，定义适合模型大小的张量竞技场。按照惯例，这个数值是通过反复试验确定的：

```
const int tensor_arena_size = 70 * 1024;
uint8_t tensor_arena[tensor_arena_size];
```

接下来，进行典型的设置工作以准备解释器，例如使用`MicroMutableOpResolver`注册所需的 ops：

```
// Set up logging.
tflite::MicroErrorReporter micro_error_reporter;
tflite::ErrorReporter* error_reporter = &micro_error_reporter;

// Map the model into a usable data structure. This doesn't involve any
// copying or parsing, it's a very lightweight operation.
const tflite::Model* model = ::tflite::GetModel(g_person_detect_model_data);
if (model->version() != TFLITE_SCHEMA_VERSION) {
error_reporter->Report(
    "Model provided is schema version %d not equal "
    "to supported version %d.\n",
    model->version(), TFLITE_SCHEMA_VERSION);
}

// Pull in only the operation implementations we need.
tflite::MicroMutableOpResolver micro_mutable_op_resolver;
micro_mutable_op_resolver.AddBuiltin(
    tflite::BuiltinOperator_DEPTHWISE_CONV_2D,
    tflite::ops::micro::Register_DEPTHWISE_CONV_2D());
micro_mutable_op_resolver.AddBuiltin(tflite::BuiltinOperator_CONV_2D,
                                     tflite::ops::micro::Register_CONV_2D());
micro_mutable_op_resolver.AddBuiltin(
    tflite::BuiltinOperator_AVERAGE_POOL_2D,
    tflite::ops::micro::Register_AVERAGE_POOL_2D());

// Build an interpreter to run the model with.
tflite::MicroInterpreter interpreter(model, micro_mutable_op_resolver,
                                     tensor_arena, tensor_arena_size,
                                     error_reporter);
interpreter.AllocateTensors();
```

下一步是检查输入张量。检查输入张量是否具有预期的维度数，以及这些维度是否大小合适：

```
// Get information about the memory area to use for the model's input.
TfLiteTensor* input = interpreter.input(0);

// Make sure the input has the properties we expect.
TF_LITE_MICRO_EXPECT_NE(nullptr, input);
TF_LITE_MICRO_EXPECT_EQ(4, input->dims->size);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(kNumRows, input->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(kNumCols, input->dims->data[2]);
TF_LITE_MICRO_EXPECT_EQ(kNumChannels, input->dims->data[3]);
TF_LITE_MICRO_EXPECT_EQ(kTfLiteUInt8, input->type);
```

从这里可以看出，输入严格来说是一个5维张量。第一个维度只是包含一个元素的包装器。接下来的两个维度表示图像像素的行和列。最后一个维度保存用于表示每个像素的颜色通道数。

表示预期尺寸的常量`kNumRows`、`kNumCols`、`kNumChannels`在[`model_settings.h`中](https://oreil.ly/ae2OI)定义：

```
constexpr int kNumCols = 96;
constexpr int kNumRows = 96;
constexpr int kNumChannels = 1;
```

如你所见，该模型接受96×96像素的位图。图像为灰度，每个像素有一个颜色通道。

接下来，使用`for`循环将测试图像复制到输入张量中：

```
// Copy an image with a person into the memory area used for the input.
const uint8_t* person_data = g_person_data;
for (int i = 0; i < input->bytes; ++i) {
    input->data.uint8[i] = person_data[i];
}
```

存储图像数据的变量`g_person_data`在`person_image_data.h`中定义。为了不向仓库增加更大的文件，数据本身在首次运行测试时作为模型的一部分与`tf_lite_micro_person_data_grayscale.zip`一起下载。

输入张量输入完成后，执行推理。与之前一样简单：

```
// Run the model on this input and make sure it succeeds.
TfLiteStatus invoke_status = interpreter.Invoke();
if (invoke_status != kTfLiteOk) {
    error_reporter->Report("Invoke failed\n");
}
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);
```

在这里检查输出张量，确认其具有预期的大小和形状：

```
TfLiteTensor* output = interpreter.output(0);
TF_LITE_MICRO_EXPECT_EQ(4, output->dims->size);
TF_LITE_MICRO_EXPECT_EQ(1, output->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(1, output->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(1, output->dims->data[2]);
TF_LITE_MICRO_EXPECT_EQ(kCategoryCount, output->dims->data[3]);
TF_LITE_MICRO_EXPECT_EQ(kTfLiteUInt8, output->type);
```

模型输出有4个维度。前三个只是第四个维度的包装器，每个类别对应一个元素，该类别是模型训练过的。

类别总数、以及其他有用值与常量`kCategoryCount`一起位于`model_settings.h`中：

```
constexpr int kCategoryCount = 3;
constexpr int kPersonIndex = 1;
constexpr int kNotAPersonIndex = 2;
extern const char* kCategoryLabels[kCategoryCount];
```
