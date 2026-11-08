# 第 7 章. 构建可扩展的深度学习和大型语言模型项目

*深度学习* (DL) 是受人类大脑结构和功能启发的一种机器学习子领域。在深度学习中，神经网络由相互连接的人工神经元层组成，这些层以分层方式处理数据，能够捕捉数据中的复杂模式。每一层都学习并转换输入数据，逐渐捕捉更高层次的特征和抽象。

DL 训练过程涉及将标记数据输入神经网络并迭代调整神经元的权重和偏置。它可以减少对手动特征工程的依赖，并在计算机视觉、自然语言处理、语音识别和强化学习等多个领域取得令人印象深刻的结果。

DL 技术即将通过变压器、生成式 AI、ChatGPT 等创新改变世界。此外，更大、更智能的基础模型可以执行类人任务、生成和理解内容等。

开发和构建深度学习模型会带来额外的运营复杂性和扩展挑战。这正是 MLOps 发挥作用的地方，帮助简化并抽象复杂性，并将开发和使用复杂模型的过程操作化。

有多个深度学习框架。主要的有：
* `TensorFlow`：由 Google 开发，[`TensorFlow`](https://www.tensorflow.org/) 是最广泛使用的深度学习框架之一。`TensorFlow` 是开源的，并提供全面的工具、库和高级 API（例如 `Keras`）生态系统，用于构建和部署深度学习模型。
* `PyTorch`：由 Meta 的 AI 研究实验室开发，[`PyTorch`](https://pytorch.org/) 是一个开源深度学习库，因其提供灵活的动态计算图而获得了显著的流行度，使得构建和训练深度学习模型变得容易。
* `Keras`：最初是一个独立库，[`Keras`](https://keras.io/) 已成为 `TensorFlow` 官方 API 的一部分。`Keras` 是开源的，并提供更简单的用于构建和训练深度学习模型的高级 API。
* `Caffe`：[`Caffe`](https://caffe.berkeleyvision.org/) 是一个由伯克利人工智能研究 (BAIR) 开发的开源深度学习框架，允许构建、训练和部署深度神经网络。`Caffe` 专注于计算机视觉任务，以其速度和效率而闻名。

这些解决方案提供多种功能和能力，包括 GPU 加速、分布式训练以及预建模型和架构，使得开发和训练复杂的深度学习模型变得更容易。

## 分布式深度学习

随着模型规模的增大和训练数据量的累积，越来越需要加速并在多台计算机上分布训练过程。分布式训练过程将任务分解为较小的任务或数据元素，并将结果组合成更大的模型。分布式（并行）训练的两种广泛使用的方法是：
* *数据并行*：将模型复制到多个系统，每个副本在数据子集上进行训练。然后对每个副本计算的梯度进行平均以更新共享模型参数。当模型参数比数据规模更重要时，数据并行是有效的。
* *模型并行*：将模型的不同部分分布到多个系统或 GPU 设备。每个系统或设备负责计算其分配的模型部分的正向和反向传递。当模型太大而无法放入单个系统或 GPU 的内存中时，使用这种方法。

图 7-1 展示了数据并行和模型并行之间的差异。

![](img/8-data-vs-model-distribution.png)

在分布式训练中，系统之间会交换大量数据，需要快速网络和高性能消息传递协议（例如 [`MPI`](https://en.wikipedia.org/wiki/Message_Passing_Interface)）。

`TensorFlow` 和 `PyTorch` 提供了用于分布式训练的内置库和解决方案。这些库可以部署在 `Kubernetes` 集群上，后者将分配工作节点。

更通用和全面的选项是在分布式计算框架上运行训练，例如 [`Horovod`](https://www.uber.com/blog/horovod/) 或 [`Ray`](https://www.ray.io/)。这两个选项都与 `TensorFlow` 和 `PyTorch` 紧密集成。

### Horovod

[`Horovod`](https://www.uber.com/blog/horovod/) 是由 Uber 开发的分布式训练框架。它支持 `TensorFlow`、`PyTorch` 和其他深度学习库。此外，`Horovod` 支持模型并行和数据并行，并实现了高效的基于环的通信，以在分布式工作节点之间同步梯度（参见 图 7-2）。它旨在高效扩展到大型集群，并广泛用于分布式深度学习。它可以使用高性能 `MPI` 通信库进行快速数据交换和同步。

![](img/8-horovod-architecture.png)

### Ray

[`Ray`](https://www.ray.io/) 是一个通用分布式计算框架，包含对分布式深度学习的支持。它提供了一套专用于 AI（AI Runtime）的工具，如用于超参数优化的 `Ray Tune` 和用于分布式深度学习训练的 `Ray Train`（参见 图 7-3）。`Ray` 设计为灵活、可扩展且易于使用，使其适用于各种分布式计算和训练场景。`Ray` 现在广泛用于数据处理、训练和服务大型语言模型（LLMs）。

![](img/8-ray-architecture.png)

> **注意**
> `MLRun` 提供了 `Horovod`/`MPI` 和 `Ray` 无服务器函数，消除了部署和扩展的复杂性。它接收代码，自动编排集群，并自动执行和跟踪分布式作业。

[`DeepSpeed`](https://www.deepspeed.ai/) 由微软研究院开发，是优化和分发大型深度学习模型训练和服务的另一个重要框架。`DeepSpeed` 在 `Ray` 或 `Horovod`/`MPI` 之上运行。

## DL 中的数据收集、标注和监控



# 用于模型训练的数据和数据标注

获取足够的模型训练数据对于构建稳健且准确的机器学习模型至关重要。然而，在获取足够的模型训练数据方面存在若干挑战。不准确的数据会产生[漂移](https://www.iguazio.com/glossary/drift-monitoring/)，这会使模型性能不可靠。以下是获取相关模型训练数据的一些关键挑战：

- *数据可用性*: 有时所需数据可能无法获得。这可能是由于各种原因，例如数据隐私、访问受限或缺乏数据收集。
- *数据质量*: 即使数据可用，其质量可能不足以用于训练模型。数据可能不完整、嘈杂或不一致，这会影响模型的性能。
- *数据多样性*: 机器学习模型需要在多样化的数据上进行训练以实现良好的泛化并避免偏差。然而，获取多样化数据具有挑战性，因为它可能需要从各种来源收集数据，这既耗时又昂贵。
- *数据不平衡*: 有时数据可能不平衡，意味着每个类别中的实例数量不相等。这可能导致有偏见的模型，在代表性不足的类别上表现不佳。
- *成本*: 收集大量数据可能很昂贵，特别是如果数据需要手动收集或通过外部来源获取。
- *时间*: 为模型训练收集足够的数据可能耗时。这可能会延迟模型的开发和部署，这对企业和组织来说是一个重大挑战。

解决这些挑战需要仔细规划和执行数据收集和预处理策略。

## 数据标注概述

*数据标注*，也称为*数据标记*，是在深度学习模型的数据准备过程中向数据样本添加元数据。数据标注有助于机器学习算法理解数据并做出准确预测。

数据标注涉及自动或半自动地为数据样本分配标签或标记，以帮助深度学习算法对数据进行分类并识别模式。这包括分配类别、添加描述性标签或对数据中的特定特征进行注释。

例如，在图像分类任务中，数据标注涉及手动为每张图像标记正确的标签，以指示图像中描绘了什么物体或场景。在*自然语言处理*（NLP）中，数据标注可能涉及为文本文档中的单个单词或短语分配类别或标签，以帮助算法识别它们之间的模式和关系。例如，照片上的标签可能识别出人脸或房屋，音频录音中说了哪些词语，或 X 光片中的肿瘤。

数据标注对于训练深度学习模型至关重要，因为它有助于算法更好地理解和识别数据中的模式。准确的标注可以提高深度学习模型的准确性和有效性，而不准确或不一致的标注可能导致预测不准确和性能下降。

数据标注最常用于：

- *计算机视觉*: 标注图像、像素、关键点或边界框。
- *NLP*: 为情感分析、实体名称识别和字符识别标记重要文本部分。
- *音频*: 将声音转换为结构化格式。

## 数据标注应避免的陷阱

在为深度学习模型标注数据时可能会出现若干潜在陷阱。一些常见陷阱包括：

- *缺乏一致性*: 标注不一致可能导致混淆并降低深度学习模型的准确性。
- *偏差*: 标注可能受到标注者个人偏见的影响，从而导致生成的模型出现偏差。例如，假设标注者偏好某个特定类别或特征。在这种情况下，他们可能会无意中以强化这种偏好方式标注数据，从而导致模型出现偏差。
- *标签不足或不相关*: 用不足或不相关信息标注数据会限制深度学习模型的有效性。例如，用“动物”这样笼统的类别标注图像可能无法为模型提供足够的信息来准确分类图像。
- *数据不足*: 缺乏足够的标注数据可能导致深度学习模型性能不佳，因为它可能没有足够示例可供学习。
- *耗时且昂贵*: 标注大量数据可能耗时且昂贵，特别是对于音频或视频等复杂数据类型。
- *过拟合*: 过拟合发生在深度学习模型在有限的标注数据上训练时，而这些数据可能无法代表所有可能数据的全范围。这可能导致模型过度专业化，在新的未见数据上表现不佳。

## 数据标注最佳实践

数据标注是构建有效深度学习模型的关键步骤。一些最佳实践可以帮助确保标注准确且一致：

- *明确定义标签*: 必须明确定义标签并为标注者提供详细说明和指导。这有助于确保所有标注者清楚理解标注任务并能产生一致且准确的标签。
- *使用多名标注者*: 使用多名标注者对每个数据样本进行注释可以帮助识别不一致并提高准确性。这可以通过让每个标注者注释数据的一个子集并比较结果来实现。
- *培训标注者*: 为标注者提供培训可以帮助确保他们理解标注任务并能产生准确且一致的标签。这可以包括对标注指南的培训、正确标注数据的示例以及对其表现的反馈。
- *监控和评估标注者*: 定期监控和评估标注者可以帮助识别不一致或错误并提高标注质量。这可以通过定期审查标注数据并向标注者提供反馈来实现。
- *使用质量控制措施*: 诸如抽查、测试集和标注者间一致性检查等质量控制措施可以帮助确保标注的准确性和一致性。
- *验证和完善标签*: 验证和完善标签可以帮助提高深度学习模型的准确性和有效性。这可以包括在标注数据上评估模型的性能并根据需要调整标注流程。
- *记录标注流程*: 记录标注流程可以帮助确保其可复制且透明。这可以包括记录标注指南、培训材料和性能评估结果。

通过遵循这些最佳实践，数据标注可以准确且一致地完成，从而获得性能更好的深度学习模型。

## 数据标注解决方案

有许多商业解决方案可用于数据标记，可帮助自动化和简化标注大型数据集的过程。以下是其中一些最受欢迎的解决方案：

- *Amazon SageMaker Ground Truth*: [亚马逊 SageMaker Ground Truth](https://aws.amazon.com/sagemaker/data-labeling/) 是一款完全托管的数据标注服务，可轻松构建用于机器学习的高度准确的训练数据集。
- *Labelbox*: [Labelbox](https://labelbox.com/) 是一个数据标注平台，提供各种工具和服务用于管理和标注大型数据集，包括图像、视频和文本数据。
- *Figure Eight*: [Figure Eight](https://appen.com/products/data-annotation/?amp)（前身为 CrowdFlower，被 Appen 收购）是一个数据标注和收集平台，提供广泛的标注服务，包括图像标注、文本标注和音频转录。



# 数据标注平台

*   *Scale AI*：[Scale AI](https://scale.com/data-labeling) 是一个数据标注平台，提供各种标注服务，包括图像和视频标注、3D 点云标注以及自然语言处理（NLP）标注。
*   *SuperAnnotate*：[SuperAnnotate](https://www.superannotate.com/) 是一个数据标注平台，提供各种工具和服务用于图像和视频标注，包括目标检测、分割和分类。
*   *Hasty*：[Hasty](https://hasty.cloudfactory.com/) 是一个数据标注平台，提供各种标注服务，包括图像和视频标注、文本标注和音频转录。

此外，还有许多用于数据标记的开源解决方案可供使用，包括：

*   *Label Studio*：[Label Studio](https://labelstud.io/) 是一个开源数据标注工具，支持各种标注任务，包括图像、文本和音频数据。它还支持多种机器学习框架，包括 PyTorch。
*   *OpenLabeling*：[OpenLabeling](https://github.com/Cartucho/OpenLabeling) 是一个开源数据标注工具，支持各种标注任务，包括目标检测、分割和分类。它还支持多种数据类型，包括图像和视频。
*   *VGG Image Annotator (VIA)*：[VGG Image Annotator](https://www.robots.ox.ac.uk/~vgg/software/via/) (VIA) 是一个开源图像标注工具，支持各种标注任务，包括目标检测、分割和分类。它还支持多种文件格式，包括 JPEG、PNG 和 TIFF。
*   *Doccano*：[Doccano](https://github.com/doccano/doccano) 是一个开源文本标注工具，支持各种标注任务，包括命名实体识别、情感分析和文本分类。它还支持多种语言和自定义标注工作流。
*   *Anno-Mage*：[Anno-Mage](https://github.com/virajmavani/semi-auto-image-annotation-tool) 是一个开源图像标注工具，支持各种标注任务，包括目标检测、分割和分类。它还支持多种标注格式，包括 Pascal、VOC 和 COCO。

图 7-4 演示了在 Label Studio 中标注图片的过程。

![](img/8-labelstudio-predict-owl-region.gif)

这些开源解决方案提供多种功能和能力，包括对多种数据类型的支持、可定制的标注工作流以及与机器学习框架的集成。它们也高度可定制，可以根据特定用例和需求进行修改。

## 使用非结构化数据监控 DL 模型

有许多简单的方法可以监控使用结构化数据的 ML 模型。例如，通过观察训练数据与生产数据之间的统计偏差来检测漂移。质量可以通过确保值和类别落在预期值集合或范围内等来观察。然而，使用非结构化数据（如文本和图像）监控模型可能更复杂。

监控非结构化数据漂移的一种方法是将其转换为结构化表示。以下是一些示例：

*   将图像转换为一组平均 RGB 颜色值或纹理，并检查颜色或纹理分布是否发生变化。
*   在图像或文本分类问题中，可以测量目标类别的分布和评分概率，或使用聚类技术并比较训练和生产之间的结果。
*   使用图像或文本嵌入并跟踪嵌入分布的变化。
*   在文本数据中，可以检查词频的变化或从文本中提取主题，并衡量主题的变化。

监控并不局限于漂移。例如，还可以监控其他模型和数据属性：

*   *性能*：衡量各种性能指标，如延迟、吞吐量、错误等。
*   *资源使用*：每个任务的内存、GPU、CPU 和 IO。
*   *准确性*：预测准确性。这通常需要人工标注将文本或图像映射到预定义类别。
*   *情感或毒性*：检查是否存在有毒语言或监控提供或响应的文本中的情感。
*   *PII（个人身份信息）违规*：检查请求或响应是否包含 PII 数据，例如信用卡号码、姓名、电子邮件、电话号码、社会保障号码等。
*   *偏见和公平性*：衡量文本中沿特定人口统计属性的偏见。

# 构建 vs 购买深度学习模型

构建用于训练深度学习模型的解决方案涉及创建定制系统，以满足组织或个人的特定需求和要求。这可能包括设计和构建定制硬件、开发定制软件和算法，以及获取和清理大型数据集。购买和构建用于训练深度学习模型的解决方案之间的关键区别是：

*   *部署时间*：购买预建解决方案可以实现更快部署，因为必要的工具和基础设施已就位。另一方面，构建定制解决方案可能需要更长时间，因为它需要设计、构建和测试必要的组件。
*   *可定制性*：构建定制解决方案允许对训练过程进行更大的控制和定制，包括灵活修改和优化算法和基础设施以适应特定需求。购买预建解决方案可能无法提供同等水平的定制。例如，集成可能具有挑战性。
*   *成本*：构建定制解决方案在前期似乎更具成本效益，但从长远来看可能更昂贵。设计、构建、维护和更新解决方案所需的资源将随着时间的推移而增加，给团队带来巨大的额外负担。基础模型和 LLM 仅训练就可能花费数千万美元。
*   *专业知识获取*：许多预建解决方案由深度学习专家开发和维护，这意味着用户可以受益于他们的专业知识和经验。内部构建意味着团队需要自行获取这些知识。
*   *数据*：大型模型使用大量数据进行训练，准备这些数据可能需要大量时间和处理，在某些情况下数据不可用或存储成本过高。
*   *降低风险*：预建解决方案通常由供应商测试和验证，这可以降低训练过程中出现错误或故障的风险。
*   *供应商依赖*：预建解决方案的用户可能依赖供应商提供支持、更新和新功能。如果供应商倒闭或停止对产品的支持，这可能会有问题。

今天有开源模型仓库（如 [Hugging Face](https://huggingface.co/)），您可以在其中找到适用于不同应用的预训练模型，而无需从供应商处购买。

还有一种不需要从头训练模型的选项。例如，您可以通过迁移学习和微调扩展商业或开源基线模型。迁移学习让您可以调整参数或权重，从而提高模型在特定任务或内容上的性能。本章后面我们将提供使用自定义数据集调优 LLM 的示例。

模型需要不断改进，通过使用新数据重新训练模型或通过 *人类反馈强化学习*（RLHF）修复模型行为。这应纳入决策考量。

# 基础模型、生成式 AI、LLM



# 基础模型与生成式 AI

## 概述

新一代基础模型已出现。诸如 `ChatGPT` 之类的应用激发了人们的想象力，也在人们心中引发了恐惧。新的技术具有变革性，很可能会在不久的将来影响我们的生活。在本节中，你将更深入地了解它们，如何使用它们，以及如何围绕它们开发解决方案。

首先，在该上下文中使用的有几个术语需要定义：

## 关键术语

*   `Foundation models (FMs)`：规模庞大的深度学习模型（例如 `GPT-3`、`GPT-4`），它们使用注意力机制在海量数据集上进行预训练，并可适应各种下游任务，包括内容生成。这与传统 AI 为特定任务设计模型的方法形成对比。
*   `Generative AI`：使用算法生成内容（例如图像、文本或音乐）的方法和工具，通常使用基础模型。
*   `LLM (Large Language Model)`：一种基础模型（如 `ChatGPT`），可以执行各种自然语言处理（`NLP`）任务。`LLMs` 在大量文本数据集上训练，学会生成类人类文本。这些模型可以理解上下文、回答问题、撰写论文、总结文本、翻译语言，甚至生成代码。

图 7-5 说明了基础模型（和 `LLMs`）的不同用例。

![](img/8-foundation-models.jpg)

## 训练与生态

基础模型是在广泛的数据语料上训练的。这可能包括 `Wikipedia` 中的全部内容、`GitHub` 上的代码以及其他公共知识来源。不幸的是，训练一个基础模型是一项成本高昂且耗时的任务。例如，据公开资料显示，最近的 `GPT-4` 模型是在拥有 25,000 个 GPU 的集群上训练的。训练周期超过一个月，据估计成本超过 $10,000,000。

今天基础模型之间存在军备竞赛。主要科技公司如 Microsoft（OpenAI）、Google、Meta、NVIDIA 以及新进入者正在开发更智能、更安全、更快或在特定领域表现更好的新模型。此外，开源 `LLM` 努力正在获得动力，在许多用例中，其智能和性能已赶上商业对应产品。因此，请确保在解决方案中引入灵活性，以便在不重构工作的情况下在基线模型之间切换。

图 7-6 显示了当前的基础模型格局（黄色表示开源模型）。

![](img/8-llm-landscape.png)

大多数组织无法承担这种奢侈，也不需要训练自己的基础模型。基础模型可以在不进行完整训练的情况下针对特定任务进行调优或定制。

## 使用方法

使用基础模型和 `LLMs` 主要有两种方法：

*   `Prompt engineering`：向模型提供经过设计的请求（提示），包括特定内容、细节、澄清说明和示例，以引导模型给出预期且最准确的答案。
*   `Transfer learning (or fine tuning)`：使用现有的预训练模型，并用特定于应用的数据集对其进行进一步训练，显著减少计算时间和成本以及训练数据集大小，并实现更好的准确性。

提示工程的优点是你无需训练或托管模型。相反，你可以通过 `API` 访问现成模型。然而，缺点是推理性能较低，且 `API` 调用成本更高（由于大型提示和提示处理开销）。此外，提示通常限制在几千个词以内，限制了输入内容的大小或导致多次 `API` 请求。也可能存在其他问题，例如保护或侵犯知识产权。

微调允许你用大量数据扩展模型的知识并将其调整到你的需求。然而，它增加了训练（调优）和验证衍生模型的复杂性和成本。此外，你需要处理部署、升级和维护等所有方面。

假设你频繁使用模型并且需要它对定义良好的内容回答更具体的问题。那么，调优和部署自己的模型是有意义的。此外，较小的开源模型通常在特定任务上表现优于较大的模型。

如果你很少使用模型，拥有专门的基础设施和 GPU 来托管模型可能过于昂贵。相反，请考虑使用现有模型的提示工程选项。

在许多情况下，基础模型和 `LLMs` 并不会产生预期结果，需要不断改进和修复。因此，你应应用诸如 `reinforcement learning from human feedback (RLHF)` 之类的技术来持续纠正和“教导”模型。不幸的是，这给项目增加了额外的复杂性和成本。

## 生成式 AI 的风险与挑战

虽然大型语言模型和 `ChatGPT` 极其有用，但它们也可能很危险。你应该意识到并作为解决方案的一部分解决的几个风险。它们可以分为以下几类：

*   `Fairness and bias`：模型基于包含固有偏见的通用互联网数据进行训练，这种偏见可能在语言中被放大或被模型误解。此外，数据可能对声音和社区的代表性不足，导致模型以偏见和不公平的方式作出回应。
*   `Intellectual property and privacy`：训练数据集来自公共来源。这可能导致结果包含敏感的、个人的或专有的信息，从而侵犯版权法和许可。
*   `Toxicity`：从互联网收集的数据包含有毒、冒犯、非道德和有害的语言。这可能导致输出包含明确或隐含的有毒内容，包括仇恨言论、骚扰、错误信息或歧视性内容。
*   `Regulatory`：当你部署或使用生成模型时，你对其所有结果以及对法律标准、合规性、知识产权、安全、安全的违反负责，等等。
*   `Misuse`：模型可被滥用来造成伤害，包括生成虚假和深度伪造内容、骚扰、虐待、冒充、恐怖、垃圾邮件和网络钓鱼。
*   `Hallucination`：模型基于统计和数学方法生成答案。它们并不“理解”内容，而是根据模式学习选择最可能的词。有大量模型听起来自信但生成虚假和不准确答案的例子。

图 7-7 和 图 7-8 展示了模型幻觉和滥用的示例。你可以注意到，已为 `GPT` 添加了安全机制以避免有毒语言或滥用。然而，这些机制尚未万无一失。

![](img/8-hallucination-example.png)![](img/8-misuse-example.png)

组织可以通过确保仅使用有效数据训练模型、实施数据保护和安全保障以及确保符合监管标准来减轻风险。此外，通过在数据处理、训练、测试和服务管道中实施安全和质量措施，可以实现更高的质量、可靠性和风险降低。

## 数据处理

数据处理和验证是构建生成式 AI 应用时最关键的任务。以下是数据处理步骤的示例：

1.  `Data cleansing`：删除不需要的文本、符号、广告、拼写和语法错误、无关信息、URL 等。使用基于启发式的方法，例如困惑度，以消除低质量文本。
2.  `Toxicity detection and filtering`：识别有毒语言、禁用词、仇恨言论和种族主义言论，并将其从训练数据集中删除，或在交互式应用中阻止该交易。




## 数据处理

3.  *偏差检测与缓解*：衡量特定人口统计属性中的偏差，并在训练集中进行平衡，或另行删除识别细节。
4.  *隐私保护*：删除任何个人身份信息（PII），包括姓名、电话号码、信用卡号码、ID、电子邮件地址等。检查私人文档或代码潜在泄露情况。
5.  *去重*：从数据集中删除重复的文本片段，以提高模型准确性并减少幻觉。
6.  *格式化与标注*：以上下文格式组织和标记数据以提高模型可靠性。例如，将文档拆分为段落或章节，并使用清晰的标题（涵盖该部分的内容）。另一个例子是将文本组织为问答或文本与类别配对。
7.  *向量嵌入与索引*：将段落或图像转换为数值向量表示以捕捉语义和句法关系，并对向量进行索引以快速识别相似性。
8.  *分词*：将文本序列分解为称为 tokens 的更小单元。这些 tokens 可以是词、下词或字符，具体取决于所选的分词策略。分词使文本能够转换为机器可以处理和分析的格式。

图 7-9 说明了 NLP（自然语言处理）应用中的数据处理流水线。

![](img/8-nlp-data-pipeline.png)

数据控制可以实现在以下任务之一或多个中：
1.  *数据预处理*：在数据发送到模型微调或向量索引流程之前进行数据清洗和准备。
2.  *模型和应用测试*：使用不同的输入提示测试模型，并验证输出和模型行为是否符合预期标准和质量。
3.  *用户请求处理*：在将请求发送给模型之前，检查传入的用户请求和提示（实时）。
4.  *验证模型响应*：在将结果返回给用户之前实时验证其合规性和质量，并阻止它们或提供替代响应。
5.  *监控应用日志*：将所有请求（提示）和响应发送到监控系统，该系统检查结果的合规性、准确性和质量。

每个方法都有优点和缺点，最好在最早阶段（数据预处理）解决该问题，但在许多情况下，由于性能或复杂性原因，无法在应用数据路径上实现解决方案，跟踪是在监控系统中完成的，其反馈用于进一步调优系统。

另一个确保高质量和可靠性的重要因素是添加用户反馈 *( *人工在环* )*。这意味着对生产数据中的结果进行采样，并由人工验证其是否符合预期行为。如果不符合，则纠正它们并重新训练或调优模型以修复该行为。

## 用于高效使用和定制 LLM 的 MLOps 流水线

本章前面我们描述了两种为特定数据和应用定制 LLM 的方法，即提示工程和微调。在许多应用中，我们会同时使用两者以最大化性能和可靠性。数据准备和验证是两种情况下流程的关键部分。

在微调中，您摄取并准备应用数据和反馈数据，运行迁移学习作业以调整基础基础模型并进行大量测试，最后将新构建的模型部署到暂存或生产环境（见 图 7-10）。

![](img/8-llm-tuning-pipe.png)

流水线可以在您有新数据或足够反馈数据（通过人工标注生成）时随时运行。此外，由于目标模型很大，需要在多个系统和 GPU 上进行分布式训练（调优）和验证，这可以使用 Horovod/MPI 或 Ray 等框架完成。

当您实现提示工程并需要用参考文档或上下文为提示提供信息时，上下文应准备并索引以实现高效检索。例如，使用嵌入和向量或关键词数据库（见 图 7-11）。此外，输入数据库并索引的数据必须彻底清理和准备，以最大化响应质量并避免风险。

![](img/8-llm-indexing-pipe.png)

多个新框架针对语言处理任务的数据处理和索引。例如，[LangChain](https://python.langchain.com/en/latest/index.xhtml)，[Llama Index](https://gpt-index.readthedocs.io/en/latest/)，[Spacy](https://spacy.io/)，和 [Unstructured](https://unstructured-io.github.io/unstructured/index.xhtml) 等。

预索引的数据和调优模型用于交互式或实时流水线，该流水线拦截用户请求并以预期答案作出响应。图 7-12 说明了实时流水线。第一步是接收和处理请求，其后是数据增强、提示工程、LLM 预测、后处理以及向用户返回响应。此外，传入数据和响应被发送到监控系统、存储并用于识别漂移、性能问题或风险。最后，部分监控数据可以发送到人工标注并用于重新调优模型（RLHF）。

![](img/8-llm-serving-pipe.png)

实时流水线引入了若干挑战：
*   相对复杂性和不同元素的集成（数据处理和增强、应用逻辑、模型预测、监控、人工反馈等）。
*   将大型模型划分为多个 GPU 设备和系统。
*   模型性能（LLM 臭名昭著地慢，可能需要多次调用才能完成单个用户流程）。
*   实时请求和响应验证以避免风险。
*   持续部署和滚动升级。

弹性无服务器框架和自动化可以降低复杂性。

## 应用示例：微调 LLM 模型

以下教程提供了使用微调的大型语言模型构建和运营智能问答应用的示例。

教程包含两个主要部分：
1.  数据准备和调优流水线
2.  实时应用和模型服务流水线

该应用使用 [Iguazio 的 MLOps 博客](https://www.iguazio.com/blog/) 的内容对 LLM 进行调优，并回答 MLOps 相关问题。它使用 [MLRun](https://www.mlrun.org/) 快速构建、运行和监控这两个流水线。

完整源代码可在以下位置找到：[`github.com/mlrun/demo-llm-tuning`](https://github.com/mlrun/demo-llm-tuning)。打开 `tutorial.ipynb` 文件以查看教程流程。

### 数据准备和调优

在本教程中，开源 LLM 模型从 [Hugging Face](https://huggingface.co/) 下载，并使用从博客页面生成的 MLOps 数据集进行微调。流水线的第一步是从互联网（博客）读取页面，将其转换为清理后的文本，并对其进行预处理以最大化模型性能。以下步骤使用 Horovod、MPI 和 [DeepSpeed](https://www.microsoft.com/en-us/research/project/deepspeed/) 在多个系统和 GPU 上扩展调优过程。最后一步评估模型。参见 图 7-13 中描述的流程。

![](img/8-training-pipeline.png)

**注意**
实际应用中可能会添加更多测试。

但首先，让我们通过加载它并提问来验证基础 LLM 对 MLOps 的了解仅限于少量。参见 示例 7-1 中的代码。



### 示例 7-1. 测试基础 LLM（在微调之前）

```
from transformers import GPT2Tokenizer, GPT2LMHeadModel, GenerationConfig, pipeline

model_name = "gpt2-medium"
tokenizer = GPT2Tokenizer.from_pretrained(model_name)
model = GPT2LMHeadModel.from_pretrained(model_name)
generation_config = GenerationConfig.from_pretrained(model_name)
generator = pipeline("text-generation", model=model, tokenizer=tokenizer)

def prompt_to_response(prompt: str) -> str:
    return generator(prompt, generation_config=generation_config, max_length=50,
                     pad_token_id=tokenizer.eos_token_id)[0]["generated_text"]

print(prompt_to_response(prompt="What is MLRun?"))

> MLRun 是一个允许你在计算机上运行简单 ML 程序的工具...
```

## 数据准备

将原始 HTML 页面输入模型很可能会导致不佳且不准确的答案。在这种情况下，数据需要清理冗余符号，拆分为具有清晰标题（主题）的文本部分，并用唯一标记进行标注以识别并防止模型幻觉。

函数 `mark_header_tags()` 在文件 `data_collection.py` 中（参见代码 示例 7-2）将 HTML 文本和标题转换为标记文本。

### 示例 7-2. 将 HTML 文本和标题转换为标记文本

```
def mark_header_tags(soup: BeautifulSoup):
    """
    Adding header token and article token prefixes to all headers in html,
    in order to parse the text later easily.

    :param soup: BeautifulSoup object of the html file
    """
    nodes = soup.find_all(re.compile("^h[1-6]$"))
    # Tagging headers in html to identify in text files:
    if nodes:
        content_type = type(nodes[0].contents[0])
        nodes[0].string = content_type(
            ARTICLE_TOKEN + normalize(str(nodes[0].contents[0]))
        )
        for node in nodes[1:]:
            if node.string:
                content_type = type(node.contents[0])
                if content_type == Tag:
                    node.string = HEADER_TOKEN + normalize(node.string)
                else:
                    node.string = content_type(HEADER_TOKEN + str(node.contents[0]))
```

函数 `convert_textfile_to_data_with_prompts()` 在文件 `data_preprocess.py` 中（参见 示例 7-3）根据每个部分的主题将文档文本转换为一组提示和答案。

### 示例 7-3. 测试基础 LLM（在微调之前）

```
def convert_textfile_to_data_with_prompts(txt_file: Path):
    """
    Formatting the html text content into prompt form.
    Each header-content in the article is an element in the list of prompts

    :param txt_file: text content as a string with tokens of headers.
    :returns: list of prompts
    """
    # Read file:
    with open(txt_file, "r") as f:
        lines = f.readlines()

    start = 0
    end = 0
    subject_idx = []
    data = []
    # Dividing text into header - paragraph prompts:
    for i, line in enumerate(lines):
        if not start and line.startswith(ARTICLE_TOKEN):
            start = i
        elif HEADER_TOKEN + END_OF_ARTICLE in line:
            end = i
            break
        if line.startswith(HEADER_TOKEN):
            subject_idx.append(i)
    article_content = lines[start:end]
    subject_idx = [subject_i - start for subject_i in subject_idx]
    article_name = article_content[0].replace(ARTICLE_TOKEN, "")
    for i, subject in enumerate(subject_idx):
        if subject + 1 in subject_idx:
            continue
        subject_data = article_content[subject].replace(HEADER_TOKEN, "")
        if i + 1 == len(subject_idx):
            content_end = len(article_content)
        else:
            content_end = subject_idx[i + 1]
        content_limits = subject + 1, content_end
        data.append(
            DATA_FORMAT.format(
                article_name,
                subject_data,
                "".join(article_content[content_limits[0] : content_limits[1]]),
            )
        )
    return data
```

## 模型微调

`train()` 函数在 `train.py` 中（参见 示例 7-4）在数据上对 LLM 进行微调。它在 OpenMPI 上运行训练，并使用 DeepSpeed 在多个工作进程之间分发模型和数据，将工作在节点和 GPU 之间进行拆分。MLRun 将自动记录整个训练过程。

TBD - 添加 train 函数的描述

### 示例 7-4. 模型微调函数（在 `train.py` 中）




```
def train(
    context: MLClientCtx,
    dataset: DataItem = None,
    pretrained_tokenizer: str = None,
    pretrained_model: str = None,
    model_class: str = None,
    tokenizer_class: str = None,
    model_name: str = "huggingface-model",
    use_deepspeed: bool = True,
):
    """
    Training a model with HuggingFace transformers api.

    :param context:              MLRun context
    :param dataset:              Training dataset uri, should be passed as input.
    :param pretrained_tokenizer: the pretrained tokenizer name.
    :param pretrained_model:     the pretrained model name to train.
    :param model_class:          The model class of the model,
                                 e.g., "transformers.GPT2LMHeadModel"
    :param tokenizer_class:      The tokenizer class of the model,
                                 e.g., "transformers.AutoTokenizer"
    :param model_name:           The name of the trained model (in the registry)
    :param use_deepspeed:        Use deepspeed lib. Recommended for fine-tuning LLMs.
    """
    torch.cuda.empty_cache()
    deepspeed_config_json = None
    if use_deepspeed:
        deepspeed_config_json = os.path.join(tempfile.mkdtemp(), "ds_config.json")
        with open(deepspeed_config_json, "w") as f:
            json.dump(DEEPSPEED_CONFIG, f)
    # Creating tokenizer:
    if tokenizer_class:
        tokenizer_class = create_class(tokenizer_class)
    else:
        tokenizer_class = AutoTokenizer

    tokenizer = tokenizer_class.from_pretrained(
        pretrained_tokenizer, model_max_length=512
    )
    tokenizer.pad_token = tokenizer.eos_token

    train_dataset = Dataset.from_pandas(dataset.as_df())

    def preprocess_function(examples):
        return tokenizer(examples["text"], truncation=True, padding=True)

    tokenized_train = train_dataset.map(preprocess_function, batched=True)
    tokenized_test = None

    data_collator_kwargs = _get_sub_dict_by_prefix(
        src=context.parameters, prefix_key=KWArgsPrefixes.DATA_COLLATOR
    )
    data_collator = DataCollatorForLanguageModeling(
        tokenizer=tokenizer, mlm=False, **data_collator_kwargs
    )

    # Parsing kwargs:
    train_kwargs = _get_sub_dict_by_prefix(
        src=context.parameters, prefix_key=KWArgsPrefixes.TRAIN
    )
    if use_deepspeed:
        train_kwargs["deepspeed"] = deepspeed_config_json
    model_class_kwargs = _get_sub_dict_by_prefix(
        src=context.parameters, prefix_key=KWArgsPrefixes.MODEL_CLASS
    )

    # Loading our pretrained model:
    model_class_kwargs["pretrained_model_name_or_path"] = (
        model_class_kwargs.get("pretrained_model_name_or_path") or pretrained_model
    )
    train_kwargs["hub_token"] = train_kwargs.get("hub_token") or pretrained_tokenizer
    if not model_class_kwargs["pretrained_model_name_or_path"]:
        raise mlrun.errors.MLRunRuntimeError(
            "Must provide pretrained_model name as "
            "function argument or in extra params"
        )
    model = create_class(model_class).from_pretrained(**model_class_kwargs)

    # Preparing training arguments:
    training_args = TrainingArguments(
        output_dir=tempfile.mkdtemp(),
        **train_kwargs,
    )

    trainer = Trainer(
        model=model,
        args=training_args,
        train_dataset=tokenized_train,
        eval_dataset=tokenized_test,
        tokenizer=tokenizer,
        data_collator=data_collator,
    )

    apply_mlrun(trainer, model_name=model_name)

    # Apply training with evaluation:
    context.logger.info(f"training '{model_name}'")
    trainer.train()

    temp_directory = tempfile.gettempdir()
    trainer.save_model(temp_directory)

    # Zip the model directory:
    shutil.make_archive(
        base_name="model",
        format="zip",
        root_dir=temp_directory,
    )

    # Log the model:
    context.log_model(
        key="model",
        db_key=model_name,
        model_file="model.zip",
        tag="",
        framework="Hugging Face",
    )
```

## 模型评估

`evaluate()` 函数在 `train.py` 中（参见 示例 7-5）使用困惑度指标评估模型。

**待定 - 有关训练评估的更多描述**

### 示例 7-5. 评估微调后的模型 (in `train.py`)

```
def evaluate(context, model_path, data: pd.DataFrame):
    """
    Evaluating the model using perplexity, for more info visit:
    https://huggingface.co/docs/transformers/perplexity

    :param context:     mlrun context
    :param model_path:  path to the model directory
    :param data:        the data to evaluate the model
    """
    # Get the model artifact and file:
    (
        model_file,
        model_artifact,
        extra_data,
    ) = mlrun.artifacts.get_model(model_path)

    # Read the name:
    model_name = model_artifact.spec.db_key

    # Extract logged model files:
    model_directory = os.path.join(os.path.dirname(model_file), model_name)
    with zipfile.ZipFile(model_file, "r") as zip_file:
        zip_file.extractall(model_directory)

    # Loading the saved pretrained tokenizer and model:
    dataset = Dataset.from_pandas(data)
    tokenizer = GPT2Tokenizer.from_pretrained(model_directory)
    model = GPT2LMHeadModel.from_pretrained(model_directory)
    encodings = tokenizer("\n\n".join(dataset["text"][:5]), return_tensors="pt")

    max_length = model.config.n_positions
    stride = 512
    seq_len = encodings.input_ids.size(1)

    nlls = []
    prev_end_loc = 0
    for begin_loc in range(0, seq_len, stride):
        end_loc = min(begin_loc + max_length, seq_len)
        trg_len = end_loc - prev_end_loc  # may be different from stride on last loop
        input_ids = encodings.input_ids[:, begin_loc:end_loc]
        target_ids = input_ids.clone()
        target_ids[:, :-trg_len] = -100

        with torch.no_grad():
            outputs = model(input_ids, labels=target_ids)

            # loss calculated using CrossEntropyLoss which averages over valid labels
            neg_log_likelihood = outputs.loss

        nlls.append(neg_log_likelihood)

        prev_end_loc = end_loc
        if end_loc == seq_len:
            break

    ppl = torch.exp(torch.stack(nlls).mean()).item()
    context.log_result("perplexity", ppl)
```

## 定义 MLRun 项目并运行管道

在 MLRun 中，管道、制品和模型都是命名项目的一部分。项目通常映射到一个 GIT 项目。它们是版本化的，可以使用单个命令部署到开发或生产环境中。此外，项目是具有严格成员资格和访问控制的管理实体。第一步是创建项目及其资源（函数、管道等）。参见 示例 7-6。

### 示例 7-6. MLRun 项目设置

```
from src.project_setup import create_and_set_project

project = create_and_set_project(
    git_source="git://github.com/mlrun/demo-llm-tuning.git#main",
    name="mlopspedia-bot",
    default_image="yonishelach/mlrun-hf",
    user_project=True,
)
```

下一步是运行管道。管道使用 `project.run()` 命令执行，并接受一组参数，可用于参数化管道并允许在不同项目中潜在重用。参见 示例 7-7。

### 示例 7-7. 运行调优管道

```
workflow_run = project.run(
    name="training_workflow",
    arguments={
        "html_links": 'https://www.iguazio.com/blog/',
        "model_name": "gpt2-medium-mlrun",
        "pretrained_tokenizer": model_name,
        "pretrained_model": model_name,
        "model_class": "transformers.GPT2LMHeadModel",
        "tokenizer_class": "transformers.GPT2Tokenizer",
        "epochs": 20,
        "use_deepspeed": True,
    },
    watch=True,
    dirty=True
)
```



管道进度可以在客户端或笔记本中交互式查看。管道结束时它会生成结果摘要（参见 图 7-14）。

![](img/8-mlrun-tune-report.png)

然而，UI 提供了更具交互性和丰富性的体验（参见 图 7-15）。

![](img/8-workflow-train.png)

`MLRun` 的独特功能之一是可以无缝地将工作负载分发到多个系统和 GPU（通过编排底层的 Kubernetes、容器和 MPI 资源）。例如，在 图 7-16（取自 `MLRun` UI）中，我们可以看到 `MLRun` 将调优任务分发到 16 个 worker 和 GPU 上。

![](img/8-workers-training.png)

管道结束时，模型会自动注册并可在应用管道中使用。

#### Note
建议添加部署测试，尝试将应用和模型管道部署到 staging 环境并对其进行详尽测试，以验证其可靠性和性能。

### Application and Model Serving Pipeline
### 应用和模型服务管道
应用管道，尤其是在 LLM 用例中，由许多步骤组成以接收和处理请求。这些包括数据丰富和预处理、提示工程、模型预测、应用控制流、安全、风险控制、数据后处理和格式化、监控等步骤。因此，构建、部署和扩展应用管道具有挑战性。此外，处理性能、安全、可用性、版本控制和滚动升级等运营方面的考虑会消耗大量工程资源并花费时间。

`MLRun` 框架使用弹性无服务器函数通过允许组合由多个自定义和内置步骤组成的图（DAG）并自动将其转换为在微服务上运行的分布式管道，来自动化应用管道的构建和部署。

本教程使用由四个步骤组成的 `MLRun` Serving Graph 实现应用管道（参见 图 7-17）：

1. Data pre-processing (`preprocess`): Fit the user prompt into the model prompt structure (“Subject - Content”).
1. 数据预处理 (`preprocess`)：将用户提示适配到模型提示结构（“主题 - 内容”）。
2. LLM prediction (`LLMModelServer`): Serve our trained model and perform inferences to generate answers.
2. LLM 预测 (`LLMModelServer`)：提供我们训练好的模型并执行推理以生成答案。
3. Post-processing (`postprocess`): Check if the model generated reliable answers and format the output.
3. 后处理 (`postprocess`)：检查模型是否生成了可靠的答案并格式化输出。
4. Toxicity filter (`ToxicityClassifierModelServer`): Use the Hugging Face Evaluate package model and perform inferences to catch toxic prompt or responses and respond with a proper answer instead.
4. 毒性过滤 (`ToxicityClassifierModelServer`)：使用 Hugging Face Evaluate 包模型并执行推理以捕获有毒的提示或响应并改为做出适当的回答。

![](img/8-serving-graph.png)

步骤 2 和 4 是通用的，可用于多个应用，而步骤 1 和 3 是特定的。你可以在 GIT 仓库中看到这些步骤的完整代码实现（在 `src/serving.py` 中）。请注意，实际应用可能包含更多步骤和逻辑。

示例 7-8 代码通过指定每个步骤的函数或类、步骤参数和图关系来定义图。`plot()` 方法允许你可视化图拓扑。

#### Example 7-8. Define the serving graph topology
#### 示例 7-8. 定义服务图拓扑
```
# Set the topology and get the graph object:
graph = serving_function.set_topology("flow", engine="async")

# Add the steps:
graph.to(handler="preprocess", name="preprocess") \
     .to("LLMModelServer",
         name="mlopspedia",
         model_class="GPT2LMHeadModel",
         tokenizer_class="GPT2Tokenizer",
         model_path=project.get_artifact_uri("gpt2-medium-mlrun"),
         use_deepspeed=True) \
     .to(handler="postprocess", name="postprocess") \
     .to("ToxicityClassifierModelServer",
         name="toxicity-classifier",
         threshold=0.7).respond()

# Plot to graph:
serving_function.plot(rankdir='LR')
```

![](img/8-serving-pipe-graph.png)

一旦实时应用图定义完成，你可以本地调试它、保存它，或通过一次 API 调用或 SDK 命令将其部署到集群，如下所示：

```
# Deploy the serving function:
deployment = mlrun.deploy_function("serving")
```

要测试已部署的应用，你可以使用 `invoke()` 方法并指定 API 参数（本例中为文本问题和模型参数）。第一个示例（参见 示例 7-9）演示了一个简单的问题和答案。

#### Example 7-9. Test the application pipeline
#### 示例 7-9. 测试应用管道
```
generate_kwargs = {"max_length": 150, "temperature": 0.9, "top_p": 0.5,
                   "top_k": 25, "repetition_penalty": 1.0}

response = serving_function.invoke(
    path='/predict',
    body={"prompt": "What is machine learning?", **generate_kwargs}
)
print(response["outputs"])
```

```
MLRun is an open source MLOps orchestration framework that streamlines the automation
of machine learning projects. MLRun empowers data scientists, data engineers and
DevOps teams to develop, deploy and manage machine learning applications faster and
more accurately.
MLRun integrates with popular tools such as Jupyter, PyCharm, Spark, Etc., and
provides a robust set of built-in orchestration and monitoring capabilities.
```

示例 7-10 发送一个有毒的问题，模型会以受控的答案做出回应。

#### Example 7-10. Try the toxic language filter
#### 示例 7-10. 尝试有毒语言过滤器
```
response = serving_function.invoke(
    path='/predict',
    body={"prompt": "You are stupid!", **generate_kwargs}
)
print(response["outputs"])
```

```
This bot does not respond to toxicity.
```

### Adding a Web Interface
### 添加 Web 界面
你可以使用 `Gradio` 快速创建 UI 来演示你的聊天应用管道的行为。示例 7-11 定义了一个聊天窗口和控制控件。

#### Example 7-11. Create the GRadio interactive UI
#### 示例 7-11. 创建 GRadio 交互式 UI




```
import json

import gradio as gr
import requests

# Get the serving url to send requests to:
serving_url = deployment.outputs["endpoint"]


def generate(prompt, temperature, max_length, top_p, top_k, repetition_penalty):
    # Build the request for our serving graph:
    inputs = {
        "prompt": prompt,
        "temperature": temperature,
        "max_length": max_length,
        "top_p": top_p,
        "top_k": top_k,
        "repetition_penalty": repetition_penalty,
    }

    # call the serving function with the request:
    resp = requests.post(serving_url, data=json.dumps(inputs).encode("utf-8"))

    # Return the response:
    return resp.json()["outputs"]


# Set up a Gradio frontend application:
with gr.Blocks(analytics_enabled=False, theme=gr.themes.Soft()) as demo:
    gr.Markdown(
        """# LLM Playground
Play with the `generate` configurations and see how they make the
LLM's responses better or worse.
"""
    )
    with gr.Row():
        with gr.Column(scale=5):
            with gr.Row():
                chatbot = gr.Chatbot()
            with gr.Row():
                prompt = gr.Textbox(
                    label="Subject to ask about:",
                    placeholder="Type a question and Enter",
                )

        with gr.Column(scale=1):
            temperature = gr.Slider(
                minimum=0,
                maximum=1,
                value=0.9,
                label="Temperature",
                info="Choose between 0 and 1",
            )
            max_length = gr.Slider(
                minimum=0,
                maximum=1500,
                value=150,
                label="Maximum length",
                info="Choose between 0 and 1500",
            )
            top_p = gr.Slider(
                minimum=0,
                maximum=1,
                value=0.5,
                label="Top P",
                info="Choose between 0 and 1",
            )
            top_k = gr.Slider(
                minimum=0,
                maximum=500,
                value=25,
                label="Top k",
                info="Choose between 0 and 500",
            )
            repetition_penalty = gr.Slider(
                minimum=0,
                maximum=1,
                value=1,
                label="repetition penalty",
                info="Choose between 0 and 1",
            )
            clear = gr.Button("Clear")

    def respond(
        prompt,
        chat_history,
        temperature,
        max_length,
        top_p,
        top_k,
        repetition_penalty,
    ):
        bot_message = generate(
            prompt, temperature, max_length, top_p, top_k, repetition_penalty
        )
        chat_history.append((prompt, bot_message))

        return "", chat_history

    prompt.submit(
        respond,
        [prompt, chatbot, temperature, max_length, top_p, top_k, repetition_penalty],
        [prompt, chatbot],
    )
    clear.click(lambda: None, None, chatbot, queue=False)
```

请在 图 7-18 中查看生成的交互式窗口。

![](img/8-gradio-qa-screen.png)

MLRun 自动化了项目的打包、交付和部署。项目可以连同代码、工作流和配置一起保存到 GIT 仓库中，并可通过单个命令或 API 调用加载到暂存或生产环境。此外，MLRun 与 GitHub Actions、Jenkins 和 GitLab CI 等 CI/CD 系统具有无缝集成，可在无需额外编码或 DevOps 活动的情况下实现完全自动化和 CI/CD。

# 摘要

在本章中，我们深入探讨了一些最前沿且具有变革性的 ML 技术：深度学习和大型语言模型（LLMs）项目。我们首先概述了基于分布式深度学习的深度学习训练过程。然后，我们讨论了用于训练的数据以及在标注时需要考虑的重要事项。以及监控过程。

在下一节中，我们描述了基础模型、GenAI 和 LLMs 的创新，并专门设立了一个章节讨论其风险。最后，我们详细解释了如何构建用于使用和定制 LLMs 的 MLOps 管道。这包括数据准备、模型调优和模型评估。本章还包括了此类管道运行方式的详尽演示。

# 批判性思考讨论问题

*   分布式深度学习的主要两种方法是什么？在什么情况下应使用每一种？
*   数据标注有哪些风险？如何克服这些风险？
*   购买深度学习模型时主要考虑因素有哪些？
*   组织如何确保在 GenAI 上训练的数据的真实性？
*   模型调优的价值是什么？

# 练习

*   选择一个开源数据标注解决方案并制定一个采用该方案的数据标注计划。
*   创建一个确保训练数据不具有毒性、偏见或安全风险的模拟流程。
*   从 Hugging Face 下载一个 LLM 模型（如果没有的话），并使用你最喜欢的网站的内容对其进行微调。
*   编写无风险模型训练的代码。
*   为 #3 中的模型构建应用程序和模型服务管道。
