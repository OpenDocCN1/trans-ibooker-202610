
* `RespondToCommand()` 参数 `score` 显示预测分数。创建在 LCD 显示屏上视觉显示分数的指示器。
* 如同秘密代码短语一样，使应用程序对“是”和“否”的特定序列做出反应。
* 使用 `yes` 和 `no` 命令控制额外的 LED 或舵机等其他组件。

# 总结
到目前为止所介绍的应用程序代码主要集中于从硬件获取数据并提取适合推理的特征。实际将数据发送到模型并执行推理的部分相对较小，与第6章中介绍的示例非常相似。

这是机器学习项目的典型例子。由于模型已经训练好，我们的工作只是持续提供适当类型的数据。如果使用 TensorFlow Lite 进行嵌入式开发，编程时间的大部分将花费在获取传感器数据、将其处理为特征以及响应模型输出上。推理本身很快也很简单。

然而，嵌入式应用程序只是整个包的一部分。[第8章](#ch08.html#chapter_training_micro_speech) 将学习如何训练自己的语音模型以识别各种单词。也将详细解释其工作原理。

# 第8章. 训练唤醒词检测模型
本作品使用 AI 翻译。欢迎提供意见和反馈：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

[第7章](#ch07.html#chapter_speech_wake_word_example) 中，我们围绕一个训练用于识别“是”和“否”的模型构建了应用程序。本章将训练一个能够识别不同单词的*新*模型。

我们的应用程序代码相当通用。只需捕获和处理音频，将其发送到 TensorFlow Lite 模型，然后根据输出执行某些操作。大多数情况下，我们不关心模型正在寻找哪个单词。也就是说，如果训练新模型，只需将其放入应用程序中即可立即运行。

训练新模型时需要考虑以下事项：

インプット
:
新模型必须使用与应用程序代码相同的预处理、相同形状和格式的输入数据进行训练。

出力
:
新模型的输出必须采用相同的格式：每个类别一个概率张量。

トレーニングデータ
:
无论选择哪个新单词，都需要大量录音，人们说出该单词的声音来训练新模型。

最適化
:
该模型必须针对内存有限的微控制器进行优化，以便在上面高效运行。

幸运的是，我们现有的模型是使用 TensorFlow 团队发布的脚本训练的，可以使用该脚本训练新模型。此外，还可以免费获得可用于训练数据的语音数据集。

下一节将介绍使用此脚本训练模型的过程。然后在[“在项目中使用模型”](#ch08.html#micro_speech_using_model_in_project) 中，将新模型集成到现有应用程序代码中。之后，在[“模型如何工作”](#ch08.html#micro_speech_how_the_model_works) 中学习模型实际如何工作。最后，在“[使用自己的数据训练”](#ch08.html#micro_speech_training_with_your_own_data) 中查看如何使用自己的数据集训练模型。

# 培养新模型
这是一个示例脚本，旨在展示如何使用 TensorFlow 构建和训练语音[识别](https://oreil.ly/E292V)模型。

使用此脚本可以非常轻松地学习语音识别模型。特别是可以做到以下几点：

* 下载包含 20 种口语的语音数据集。
* 选择用于训练模型的单词子集。
* 指定用于音频的预处理类型。
* 从几种模型架构中选择。
* 使用量化为微控制器优化模型。

运行脚本后，它会下载数据集、训练模型并输出表示训练后模型的文件。接下来，使用其他工具将此文件转换为 TensorFlow Lite 的正确格式。

#### 注
模型创建者通常会创建这种类型的训练脚本。这使得可以轻松尝试模型架构和超参数的各种变体，并与其他人分享自己的工作。

运行训练脚本最简单的方法是在 Colaboratory（Colab）笔记本中运行。

## 在 Colab 中训练
Google Colab 是训练模型的理想场所。可以利用云强大的计算资源，并且已设置用于监控训练过程的工具。而且完全免费。

本节介绍如何使用 Colab 笔记本训练新模型。使用的笔记本位于 TensorFlow 仓库中。

如[图8-](#ch08.html#run_in_google_colab_2)1所示，打开[笔记本](https://oreil.ly/0Z2ra)，点击“Run in Google Colab”按钮。

![](assets/timl_0403.png)

#### 提示
在撰写本文时，GitHub 上存在一个 bug，会在显示 Jupyter 笔记本时间歇性地出现错误消息。如果显示“Sorry, something went wrong.是否重新加载？”的消息，请按照["Building Our Model "](#ch04.html#ch4_building_our_model)的说明进行操作。

此笔记本解释了训练模型的过程。此笔记本执行以下步骤：

* 设置参数
* 安装正确的依赖项
* 使用 TensorBoard 监控训练
* 运行训练脚本
* 将训练输出转换为模型，以便可以像这样使用。

### 启用 GPU 训练
[第4章](#ch04.html#chapter_hello_world_training) 中，我们使用少量数据训练了一个非常简单的模型。现在训练的模型更精细，数据集更大，训练需要更长时间。在平均现代计算机的 CPU 上，训练可能需要 3 到 4 小时。

为缩短模型训练时间，可以使用称为*GPU 加速*的东西。*GPU* 是图形处理单元，是计算机中设计用于快速处理图像数据以便平滑渲染用户界面和视频游戏等内容的硬件部分。大多数计算机都配备了它。

图像处理中许多任务是并行执行的。深度学习网络的训练也是如此。也就是说，可以使用 GPU 硬件加速深度学习训练。通常，使用 GPU 运行训练比 CPU 快 5 到 10 倍。

由于训练过程中需要音频预处理，可能无法实现如此大幅的加速，但即使如此，使用 GPU 也会使模型训练明显更快。

幸运的是，Colab 支持使用 GPU 进行训练。默认情况下未启用，但打开开关很简单。为此，请转到 Colab 的运行时菜单，如[图8-](#ch08.html#change_runtime_type)2所示，点击“Change runtime type”。

![](assets/timl_0802.png)

选择此选项后，将打开如[图8-](#ch08.html#notebook_settings)3所示的“Notebook settings”框。

![](assets/timl_0803.png)

如[图 8-4](#ch08.html#hardware_accelerator)所示，从“Hardware accelerator”下拉列表中选择 GPU，然后点击“SAVE”。

![](assets/timl_0804.png)

从现在起，Colab 将在配备 GPU 的后端计算机（称为*运行时*）上运行 Python。

下一步是在笔记本中设置要训练的单词。

### 设置训练
学习脚本由一组命令行标志控制，从模型架构到用于分类的训练单词。

为了便于运行脚本，笔记本的第一个单元格将一些重要值存储在环境变量中。当脚本运行时，这些值将被分配给脚本的命令行标志。

第一个`WANTED_WORDS` 可以选择用于训练模型的单词：

```
os.environ["WANTED_WORDS"] = "yes,no"
```

默认选择“yes”和“no”，但可以自由组合数据集中以下单词：

* 常用命令：*是*、*否*、*上*、*下*、*左*、*右*、*开*、*关*、*停止*、*Go*、*后退*、*前进*、*跟随*、*学习*
* 0 到 9 的数字：*零*、*1*、*2*、*3*、*4*、*5*、*6*、*7*、*8*、*9*
* 喜欢的词：*床*,*鸟*,*猫*,*狗*,*快乐*,*家*,*马文*,*希拉*,*树*,*哇*

要选择单词，只需将其放入逗号分隔的列表中。让我们尝试为新模型训练选择单词“on”和“off”：

```
os.environ["WANTED_WORDS"] = "on,off"
```

未包含在列表中的单词在模型训练时将被归类为“未知”。

#### 注
在这里选择两个或更多单词是可以的。只需稍微修改应用程序代码即可。有关此方法，请参阅[“在项目中使用模型”](#ch08.html#micro_speech_using_model_in_project)。

`TRAINING_STEPS` 和`LEARNING_RATE` 变量：

```
os.environ["TRAINING_STEPS"]="15000,3000"
os.environ["LEARNING_RATE"]="0.001,0.0001"
```

[第3章](#ch03.html#chapter_get_up_to_speed) 中，我们学习了模型的权重和偏差会逐步调整，随着时间的推移，模型输出会接近期望值。`TRAINING_STEPS` 指的是训练数据批次通过网络运行并更新权重和偏差的次数。`LEARNING_RATE` 设置调整率。

如果学习率较高，则权重和偏差每次迭代都会被调整，因此收敛更快。然而，这样的较大跳跃意味着可能会持续跳过理想值，因此难以到达理想值。如果学习率较低，则跳跃会变小。收敛需要更多步骤，但最终结果可能更好。给定模型的最佳学习率是通过反复试验确定的。

在上述变量中，训练步数和学习率被定义为逗号分隔的列表，定义每个训练阶段的学习率。以先前的值为例，模型以学习率 0.001 训练 15,000 步，以学习率 0.0001 训练 3,000 步。总共 18,000 步。

也就是说，使用较高的学习率进行多次迭代，使网络快速收敛。然后使用较低的学习率进行较少次数的迭代，对权重和偏差进行微调。

目前保持这些值不变，但了解这些值的作用是好的。试运行单元格。以下输出将显示：

```
Training these words: on,off
Training steps in each stage: 15000,3000
Learning rate in each stage: 0.001,0.0001
Total number of training steps: 18000
```

这是模型如何被训练的概述。

### 安装依赖项
接下来，获取运行脚本所需的几个依赖项。

运行以下两个单元格以执行以下操作：

* 安装包含训练所需 ops 的 TensorFlow `pip` 包的特定版本。
* 克隆 TensorFlow GitHub 仓库的对应版本，以便可以访问训练脚本。

### 加载 TensorBoard
为了监控训练过程，我们使用[TensorBoard](https://oreil.ly/wginD)。这是一个用户界面，可以显示训练的进展、图表、统计数据和其他见解。

训练完成后，将类似于[图8-](#ch08.html#tensorboard_complete)5的屏幕截图。这些图表的含义将在本章后面解释。

![](assets/timl_0805.png)

运行下一个单元格加载 TensorBoard。它将在 Colab 中显示，但在开始训练之前不会显示任何内容。

### 开始训练
下一个单元格运行启动训练的脚本。你会看到有很多命令行参数：

```
!python tensorflow/tensorflow/examples/speech_commands/train.py \
--model_architecture=tiny_conv --window_stride=20 --preprocess=micro \
--wanted_words=${WANTED_WORDS} --silence_percentage=25 --unknown_percentage=25 \
--quantize=1 --verbosity=WARN --how_many_training_steps=${TRAINING_STEPS} \
--learning_rate=${LEARNING_RATE} --summaries_dir=/content/retrain_logs \
--data_dir=/content/speech_dataset --train_dir=/content/speech_commands_train
```

有些参数使用之前定义的環境变量来设置要创建的模型，例如`--wanted_words=${WANTED_WORDS}`。还有一些参数设置脚本的输出，例如`--train_dir=/content/speech_commands_train`。

保持参数不变，运行单元格。你会看到输出流出。在语音命令数据集下载期间，它会暂停一段时间：

```
>> Downloading speech_commands_v0.02.tar.gz 18.1%
```

完成后，会显示更多输出。可能会出现警告，但只要单元格继续运行就可以忽略。在此点，回滚到 TensorBoard，应如下[图 8-6](#ch08.html#tensorboard_start)所示。如果未显示图表，请点击 SCALARS 选项卡。

![](assets/timl_0806.png)

耶！这意味着训练已开始。现在运行的单元格将在训练期间继续运行。单元格不再输出日志，但有关训练运行的数据将在 TensorBoard 中显示。
