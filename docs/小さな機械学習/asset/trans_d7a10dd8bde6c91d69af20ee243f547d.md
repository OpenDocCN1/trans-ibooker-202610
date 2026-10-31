
# 训练设置

- 检查点和摘要保存在通过参数给定的文件夹中。保存在通过 `--train_dir` 参数给定的文件夹中。这里需要查找结果。
- `--dataset_dir` 参数必须与 Visual Wake Words 构建脚本保存 TFRecords 的位置一致。
- 使用的架构由参数定义。由 `--model_name` 参数定义。`mobilenet_v1` 前缀指示脚本使用 MobileNet 的第一个版本。也尝试过之后的版本，但由于中间激活缓冲区使用了更多的 RAM，目前仍坚持使用原版。`025` 是使用的深度乘数，主要影响权重参数的数量。这一较低的设置使模型可设置在 250KB 的闪存中。
- `--preprocessing_name` 控制输入图像在输入模型之前如何被修改。`mobilenet_v1` 版本中，将图像的宽度和高度缩小到 `--train_image_size` 指定的大小（为了减少计算量，这里为 96 像素）。此外，将像素值从 0～255 的整数缩放为 -1.0～+1.0 的浮点数（不过在 pass:\[training\] 之后进行量化）。
- SparkFun Edge 板上使用的 [HM01B0相机是](https://oreil.ly/RGciN) 单色的，因此为了获得最佳结果，需要使用单色图像训练模型。为了启用该预处理，请传递 `--use_grayscale` 标志。
- `--learning_rate`、`--label_smoothing`、`--learning_rate_decay_factor`、`--num_epochs_per_decay`、`--moving_average_decay`、`--batch_size` 的参数都控制学习过程中权重如何更新。深度网络的训练仍然有点像黑暗艺术，这些精确的值是通过实验为此特定模型发现的。通过微调这些参数可以加快学习速度或略微提高精度，但如何更改并没有太多参考，而且很容易出现导致学习精度不收敛的组合。
- `--max_number_of_steps` 定义训练持续到何时。事先设定此阈值没有好方法。需要通过实验判断模型精度何时不再提高，以及何时应停止学习。在我们这里，默认设置为 100 万步。因为对于此特定模型，已知这是停止的良好点。

启动脚本时，应显示如下输出：

```
INFO:tensorflow:global step 4670: loss = 0.7112 (0.251 sec/step)
  I0928 00:16:21.774756 140518023943616 learning.py:507] global step 4670: loss
  = 0.7112 (0.251 sec/step)
INFO:tensorflow:global step 4680: loss = 0.6596 (0.227 sec/step)
  I0928 00:16:24.365901 140518023943616 learning.py:507] global step 4680: loss
  = 0.6596 (0.227 sec/step)
```

请忽略行的重复。这只是 TensorFlow 日志输出与 Python 交互时的副作用。每行包含有关训练过程的两个重要信息。全局步数是训练进展的计数。由于设置为 100 万步，在此情况下已完成约 5%。结合步/秒的估计，可以方便地估算整个训练过程的大致持续时间。在此情况下，每秒完成约 4 步，因此 100 万步需要约 70 小时，即 3 天。另一个重要信息是损失。它是衡量部分训练模型的预测值与正确值有多接近的指标，值越小越好。它显示出很大的波动，但如果模型正在学习，平均而言训练过程中应会下降。由于噪声很大，短时间内数值会大幅波动，但如果进展顺利，等待约 1 小时后检查应能看到明显下降。尝试 TensorBoard 的主要原因之一是可以以图表形式查看此类波动。

# TensorBoard

TensorBoard 是可视化 TensorFlow 训练会话数据的 Web 应用程序，默认包含在大多数云实例中。如果使用 Google Cloud AI Platform，可以从笔记本界面左侧的选项卡打开命令调色板，向下滚动并选择 “Create a new tensorboard ” 来启动新的 TensorBoard 会话。然后会提示输入摘要日志的保存位置。输入在训练脚本中用于 `--train_dir` 的路径。之前的示例中，文件夹名为 `vww_96_grayscale`。常见的错误是在路径末尾添加斜杠，这会导致 TensorBoard 无法发现目录。

在其他环境中从命令行启动 TensorBoard 时，需要将路径传递给 TensorBoard 命令行工具的参数 `--logdir`，浏览器为 [`http://localhost:6006`](http://localhost:6006)（或运行机器的地址）。

移动到 TensorBoard 地址或通过 Google Cloud 打开会话时，应显示类似 [图10-12的](#ch10.html#tensorboard_graphs_10) 的页面。脚本每 5 分钟仅保存一次摘要，因此图表显示有用的内容可能需要一些时间。 [图10-12是](#ch10.html#tensorboard_graphs_10) 显示训练超过一天后的结果。最重要的图表称为 `clone_loss`，它显示与日志输出中显示的相同损失值的进展。如示例所示，它波动很大，但总体趋势随时间下降。如果数小时训练后看不到此类进展，则是模型未收敛到良好解的良好迹象，可能数据集或训练参数存在问题，需要调试。

打开 TensorBoard 时，默认显示 `SCALARS` 选项卡，但在训练期间有用的其他部分是 `IMAGES` [（图10-13](#ch10.html#tensorboard_images)）。它显示模型当前正在学习的图像的随机选择，包括失真和其他预处理。在此图中，可以看到图像被翻转，并在发送到模型之前转换为灰度。此信息不如损失图重要，但有助于确认数据集符合预期，并且很有趣可以看到样本随着训练的进行而更新。

![](assets/timl_1012.png)![](assets/timl_1013.png)

# 评估模型

损失函数与模型训练得有多好相关，但不是可以直接理解的指标。我们真正关心的是模型正确检测了多少人，但需要运行另一个脚本来计算。这不需要等待模型完全训练完成。可以检查 `--train_dir` 文件夹中检查点的精度。为此，请运行以下命令：

```
! python models/research/slim/eval_image_classifier.py \
    --alsologtostderr \
    --checkpoint_path=vww_96_grayscale/model.ckpt-698580 \
    --dataset_dir=data/visualwakewords \
    --dataset_name=visualwakewords \
    --dataset_split_name=val \
    --model_name=mobilenet_v1_025 \
    --preprocessing_name=mobilenet_v1 \
    --use_grayscale=True \
    --train_image_size=96
```

需要确保 `--checkpoint_path` 指向有效的检查点数据集。检查点分为三个文件保存，因此其值必须是共同前缀。例如，如果存在检查点文件 `model.ckpt-5179.data-00000-of-00001`，则前缀为 `model.ckpt-5179`。脚本将生成如下输出：

```
INFO:tensorflow:Evaluation [406/406]
I0929 22:52:59.936022 140225887045056 evaluation.py:167] Evaluation [406/406]
eval/Accuracy[0.717438412]eval/Recall_5[1]
```

这里重要的数值是精度。它显示正确分类的图像比例，转换为百分比后，此处为 72%。按照示例脚本，完全训练的模型在 100 万步后预计可达到约 84% 的精度，并显示约 0.4 的损失。

# 将模型导出到 TensorFlow Lite

模型训练到令人满意的精度后，需要将结果从 TensorFlow 训练环境转换为可在嵌入式设备上运行的形式。如前一章所述，这可能是一个复杂的过程，而 `tf.slim` 也增加了一些皱褶。

## 导出到 GraphDef Protobuf 文件

Slim 每次运行脚本时都会从 `model_name` 生成架构，因此要在 Slim 外部使用模型，需要以通用格式保存。这里使用 Slim 和 TensorFlow 都能理解的 GraphDef protobuf 序列化格式：

```
! python models/research/slim/export_inference_graph.py \
    --alsologtostderr \
    --dataset_name=visualwakewords \
    --model_name=mobilenet_v1_025 \
    --image_size=96 \
    --use_grayscale=True \
    --output_file=vww_96_grayscale_graph.pb
```

如果成功，新的 `vww_96_grayscale_graph.pb` 文件应出现在主目录中。该文件包含模型中运算的布局，但尚未包含权重数据。

## 冻结权重

将学习到的权重与运算图一起保存的过程称为冻结。这是在从检查点文件加载值后，将图中的所有变量转换为常量。以下命令使用第 100 万次训练步的检查点，但可以指定有效的检查点路径。图冻结脚本存储在 TensorFlow 主仓库中，因此在运行此命令之前需要从 GitHub 下载：

```
! git clone https://github.com/tensorflow/tensorflow
! python tensorflow/tensorflow/python/tools/freeze_graph.py \
    --input_graph=vww_96_grayscale_graph.pb \
    --input_checkpoint=vww_96_grayscale/model.ckpt-1000000 \
    --input_binary=true --output_graph=vww_96_grayscale_frozen.pb \
    --output_node_names=MobilenetV1/Predictions/Reshape_1
```

之后应出现 `vww_96_grayscale_frozen.pb` 文件。

## 量化和转换为 TensorFlow Lite

量化是一个棘手且复杂的过程，仍是研究领域，因此将迄今学习到的浮点图转换为 8 位实体需要相当多的代码。关于量化是什么以及如何工作，请参阅 [第15章](#ch15.html#optimizing_latency)。代码的大部分用于准备要输入到已训练网络的样本图像，以便能够衡量典型使用中激活层的范围。推断引擎所需的量化和转换为 TensorFlow Lite FlatBuffer 文件依赖于 `TFLiteConverter` 类：

```
import tensorflow as tf
import io
import PIL
import numpy as np

def representative_dataset_gen():

  record_iterator = tf.python_io.tf_record_iterator
      (path='data/visualwakewords/val.record-00000-of-00010')

  count = 0
  for string_record in record_iterator:
    example = tf.train.Example()
    example.ParseFromString(string_record)
    image_stream = io.BytesIO
        (example.features.feature['image/encoded'].bytes_list.value[0])
    image = PIL.Image.open(image_stream)
    image = image.resize((96, 96))
    image = image.convert('L')
    array = np.array(image)
    array = np.expand_dims(array, axis=2)
    array = np.expand_dims(array, axis=0)
    array = ((array / 127.5) - 1.0).astype(np.float32)
    yield([array])
    count += 1
    if count > 300:
        break

converter = tf.lite.TFLiteConverter.from_frozen_graph \
    ('vww_96_grayscale_frozen.pb', ['input'],  ['MobilenetV1/Predictions/ \
    Reshape_1'])
converter.inference_input_type = tf.lite.constants.INT8
converter.inference_output_type = tf.lite.constants.INT8
converter.optimizations = [tf.lite.Optimize.DEFAULT]
converter.representative_dataset = representative_dataset_gen

tflite_quant_model = converter.convert()
open("vww_96_grayscale_quantized.tflite", "wb").write(tflite_quant_model)
```

## 转换为 C 源文件

转换器会写入文件，但大多数嵌入式设备没有文件系统。要从程序访问序列化数据，需要将其编译到可执行文件中并存储在闪存中。最简单的方法是像前一章那样将文件转换为 C 数据数组：
