
しかし、よく見ると、いくつかの不完全な部分があることがわかる。予測された正弦波のピークと谷は、実際の正弦波のように完全に滑らかではない。ランダムに分布している学習データのばらつきが、モデルに学習されてしまったのだ。滑らかな正弦関数を学習する代わりに、モデルはデータの形状を正確に再現するように学習したのだ。

我々の目的では、このオーバーフィッティングは大きな問題ではない。我々の目標は、このモデルがLEDを緩やかにフェードオン／オフさせることであり、これを達成するために完璧に滑らかである必要はない。もしオーバーフィッティングのレベルが問題だと考えるなら、正則化のテクニックや、より多くのトレーニングデータを得ることで対処することができるだろう。

モデルには満足したので、デバイス上でデプロイできるようにしよう ！

# 将模型转换为 TensorFlow Lite 用

本章开头简要提到了 TensorFlow Lite。TensorFlow Lite 是一个用于在“边缘设备”（指从手机到微控制器板等所有设备）上运行 TensorFlow 模型的工具集。

[第13章](#ch13.html#chapter_tensorflow_lite_for_microcontrollers) 将详细介绍 TensorFlow Lite for Microcontrollers。目前，可以认为 TensorFlow Lite 有两个主要组件：

TensorFlow Lite 转换器

: 

这是将 TensorFlow 模型转换为可在内存受限设备上使用的、空间高效的特殊格式，并可应用优化以进一步减小模型大小，使其在小型设备上也能高速运行。

TensorFlow Lite 解释器

: 

这是使用针对给定设备最有效的运算符来运行已正确转换的 TensorFlow Lite 模型。

在使用 TensorFlow Lite 之前需要转换模型。为此使用 TensorFlow Lite Converter 的 Python API。它接收 `Keras` 模型并以 `FlatBuffer` 格式写入磁盘。`FlatBuffer` 是一种为提高空间效率而设计的特殊文件格式。由于要部署到内存有限的设备上，这非常方便！关于 FlatBuffer 将在 [第12章](#ch12.html#chapter_magic_wand_training) 中详细介绍。

TensorFlow Lite 转换器除了创建 `FlatBuffer` 外，还可以对模型应用优化。这些优化通常会减少模型的尺寸、执行时间或两者兼有。这有时会以精度下降为代价，但通常其影响非常小。关于优化将在 [第13章](#ch13.html#chapter_tensorflow_lite_for_microcontrollers) 中详细介绍。

最有用的优化之一是 `量子化`。默认情况下，模型的权重和偏差以 32 位浮点数保存，以便在训练期间进行高精度计算。通过量化，可以降低这些数值的精度，使其适合 8 位整数。更妙的是，CPU 计算整数比浮点数更简单，因此量化后的模型运行速度更快。

量化的最棒之处在于，通常精度损失被最小化。也就是说，在部署到低内存设备时几乎总是物有所值。

在下一个单元格中，使用转换器创建并保存模型的新版本两个。第一个转换为 TensorFlow Lite FlatBuffer 格式，但不进行优化。第二个是量化的。

运行单元格，将模型转换为这两种变体：

```
# Convert the model to the TensorFlow Lite format without quantization
converter = tf.lite.TFLiteConverter.from_keras_model(model_2)
tflite_model = converter.convert()

# Save the model to disk
open("sine_model.tflite", "wb").write(tflite_model)

# Convert the model to the TensorFlow Lite format with quantization
converter = tf.lite.TFLiteConverter.from_keras_model(model_2)
# Indicate that we want to perform the default optimizations,
# which include quantization
converter.optimizations = [tf.lite.Optimize.DEFAULT]
# Define a generator function that provides our test data's x values
# as a representative dataset, and tell the converter to use it
def representative_dataset_generator():
  for value in x_test:
    # Each scalar value must be inside of a 2D array that is wrapped in a list
    yield [np.array(value, dtype=np.float32, ndmin=2)]
converter.representative_dataset = representative_dataset_generator
# Convert the model
tflite_model = converter.convert()

# Save the model to disk
open("sine_model_quantized.tflite", "wb").write(tflite_model)
```

为了创建尽可能高效运行的量化模型，需要提供 `代表的なデータセット`，即表示模型所训练的数据集输入值的全部范围的数值集合。

在前一个单元格中，可以使用测试数据集的 `x` 值作为代表数据集。定义一个名为 `representative_dataset_generator()` 的函数，使用 `yield` 运算符逐个返回値。

为了证明这些模型在转换和量化后仍然准确，使用两个模型进行预测并与测试结果进行比较。由于它们是 TensorFlow Lite 模型，因此需要使用 TensorFlow Lite 解释器。

TensorFlow Lite 解释器主要为效率而设计，因此使用起来比 `Keras` API 稍微复杂一些。对于 `Keras` 模型进行预测，只需调用 `predict()` 方法并传递输入数组即可。在 TensorFlow Lite 中，需要执行以下操作：

1. 实例化 `Interpreter` 对象。
2. 调用一些为模型分配内存的方法。
3. 将输入写入输入张量。
4. 调用模型。
5. 从输出张量读取输出。

这听起来很麻烦，但现在不用太担心。先执行下面的单元格，使用两个模型进行预测并绘制图表：

```
# Instantiate an interpreter for each model
sine_model = tf.lite.Interpreter('sine_model.tflite')
sine_model_quantized = tf.lite.Interpreter('sine_model_quantized.tflite')

# Allocate memory for each model
sine_model.allocate_tensors()
sine_model_quantized.allocate_tensors()

# Get indexes of the input and output tensors
sine_model_input_index = sine_model.get_input_details()[0]["index"]
sine_model_output_index = sine_model.get_output_details()[0]["index"]
sine_model_quantized_input_index = sine_model_quantized.get_input_details()[0]["index"]
sine_model_quantized_output_index = \
  sine_model_quantized.get_output_details()[0]["index"]

# Create arrays to store the results
sine_model_predictions = []
sine_model_quantized_predictions = []

# Run each model's interpreter for each value and store the results in arrays
for x_value in x_test:
  # Create a 2D tensor wrapping the current x value
  x_value_tensor = tf.convert_to_tensor([[x_value]], dtype=np.float32)
  # Write the value to the input tensor
  sine_model.set_tensor(sine_model_input_index, x_value_tensor)
  # Run inference
  sine_model.invoke()
  # Read the prediction from the output tensor
  sine_model_predictions.append(
      sine_model.get_tensor(sine_model_output_index)[0])
  # Do the same for the quantized model
  sine_model_quantized.set_tensor\
  (sine_model_quantized_input_index, x_value_tensor)
  sine_model_quantized.invoke()
  sine_model_quantized_predictions.append(
      sine_model_quantized.get_tensor(sine_model_quantized_output_index)[0])


# See how they line up with the data
plt.clf()
plt.title('Comparison of various models against actual values')
plt.plot(x_test, y_test, 'bo', label='Actual')
plt.plot(x_test, predictions, 'ro', label='Original predictions')
plt.plot(x_test, sine_model_predictions, 'bx', label='Lite predictions')
plt.plot(x_test, sine_model_quantized_predictions, 'gx', \
  label='Lite quantized predictions')
plt.legend()
plt.show()
```

运行此单元格后，将得到 [图4-21的](#ch04.html#test_predicted_actual_converted) 图表。

![](assets/timl_0421.png)

从图表中可以看出，原模型、转换后模型和量化模型的预测都非常接近，几乎无法区分。情况良好！

由于量化会使模型变小，让我们比较转换后的两个模型以查看大小差异。执行以下单元格计算大小并进行比较：

```
import os
basic_model_size = os.path.getsize("sine_model.tflite")
print("Basic model is %d bytes" % basic_model_size)
quantized_model_size = os.path.getsize("sine_model_quantized.tflite")
print("Quantized model is %d bytes" % quantized_model_size)
difference = basic_model_size - quantized_model_size
print("Difference is %d bytes" % difference)
```

应该会显示如下输出：

```
Basic model is 2736 bytes
Quantized model is 2512 bytes
Difference is 224 bytes
```

量化后的模型比原始版本小 224 字节。约 2.4 千字节，该模型已经非常小，权重和偏差只占整体大小的一小部分。模型不仅包含权重，还包含构成深度学习网络架构的所有逻辑，称为 `計算グラフ`。对于真正的小型模型，这可能比模型权重更大，这意味着量化几乎没有效果。

更复杂的模型具有更多权重，因此量化带来的空间节省会更大。对于最复杂的模型，预计接近 4 倍。

无论确切大小如何，量化模型比原始版本执行时间更短。

## 转换为 C 文件

准备可在 TensorFlow Lite for Microcontrollers 中使用的模型的最后一步是将模型转换为可包含在应用程序中的 C 源文件。

到目前为止，本章一直使用 TensorFlow Lite 的 Python API。也就是说，可以使用 `Interpreter` 构造函数从磁盘加载模型文件。

然而，大多数微控制器没有文件系统，即使有，考虑到有限的空间，从磁盘加载模型所需的额外代码也是浪费的。相反，作为一种优雅的解决方案，提供 C 源文件形式的模型，可将其包含在二进制文件中并直接加载到内存中。

在文件中，模型被定义为字节数组。幸运的是，有一个方便的 Unix 工具 `xxd`，可以将给定文件转换为所需格式。

下一个单元格对量化后的模型运行 `xxd`，将输出写入名为 `sine_model_quantized.cc` 的文件并打印到屏幕上：

```
# Install xxd if it is not available
!apt-get -qq install xxd
# Save the file as a C source file
!xxd -i sine_model_quantized.tflite > sine_model_quantized.cc
# Print the source file
!cat sine_model_quantized.cc
```

输出非常长，因此在此不会全部重现，但这里包含开头和结尾的片段：

```
unsigned char sine_model_quantized_tflite[] = {
  0x1c, 0x00, 0x00, 0x00, 0x54, 0x46, 0x4c, 0x33, 0x00, 0x00, 0x12, 0x00,
  0x1c, 0x00, 0x04, 0x00, 0x08, 0x00, 0x0c, 0x00, 0x10, 0x00, 0x14, 0x00,
  // ...
  0x00, 0x00, 0x08, 0x00, 0x0a, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x09,
  0x04, 0x00, 0x00, 0x00
};
unsigned int sine_model_quantized_tflite_len = 2512;
```

要在项目中使用此模型，可以复制并粘贴源代码，或从笔记本下载文件。

## 总结

现在模型的构建已完成。TensorFlow 深度学习网络可以接收 0 到 2π 之间的数值，并输出其正弦的足够近似值。
