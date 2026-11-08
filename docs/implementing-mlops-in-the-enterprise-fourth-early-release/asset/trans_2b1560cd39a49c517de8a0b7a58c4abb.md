
# 可扩展性和框架示例

构建和部署模型的每个步骤都需要大量的可扩展资源，例如 `S3`，用于存储原始数据和模型占位符。训练作业也需要弹性资源，推理作业也是如此。MLOps 是一个分布式计算问题，处理平台是解决这个困难问题最合理的方法之一。

另一个关于可扩展性至关重要的绝佳例子是苹果的 [`CreateML` 工具](https://developer.apple.com/machine-learning/create-ml/)。在 [图 5-14](#ch05.xhtml#Figure-5.4.2-createml) 中，来自 Kaggle 的 [狗和猫数据集](https://www.kaggle.com/c/dogs-vs-cats) 被拖放到 UI 上以设置训练作业。

![](assets/5.4.2-create-ml.png)

接下来，配备 38 个 GPU 内核和 10 个 CPU 内核的 Mac Pro M2 Max 可以充分利用这些 GPU 内核[图 5-15](#ch05.xhtml#Figure-5.4-create-ml-saturate)。这里的工具是关键，因为软件和硬件的快速融合使开发人员能够快速原型化模型，然后再从 [`CoreML`](https://developer.apple.com/documentation/coreml) 格式导出到 `ONNX`。

![](assets/5.4-create-ml-saturate.jpg)

另一个高级框架的例子是 [`ML.NET` 框架](https://dotnet.microsoft.com/en-us/apps/machinelearning-ai/ml-dotnet) 在 [图 5-16](#ch05.xhtml#Figure-5.18-mlnet) 中显示，它在执行 AutoML 时饱和了 CPU 内核。

![](assets/5.18-mlnet.png)

最后，不仅创建了一个高效使用 `ONNX` 的模型，而且 `C#` 控制台应用程序也是框架构建过程的一部分，使其能够与 `C#`、Visual Studio Code、模型格式以及最终的一次构建、部署工具紧密集成。

在 MLOps 领域，存在许多将硬件、软件、模型格式和最终交付物耦合在一起的工具的出现示例，值得为贵组织的目标评估这些框架。

## AWS 硅创新

MLOps 中的另一个拼图是 [自定义硅](https://aws.amazon.com/silicon-innovation/)。在训练方面，有 [`Tranium`](https://aws.amazon.com/machine-learning/trainium/)，一种高性能机器学习训练加速器，旨在加速深度学习工作负载。第一个版本使用约 50% 更少的能源，训练速度快 2 倍以上，训练成本降低 70% 以上，而第二个版本吞吐量高出 4 倍，延迟降低 10 倍，并且支持超大生成式 AI 模型。

在推理方面，还有 [`AWS Inferentia`](https://aws.amazon.com/machine-learning/inferentia/) 这种性能意味着 `OPT-30B` 模型（300 亿参数的大型语言模型）比基于 GPU 的推理快 3 倍以上，而 `OPT-66B` 在 GPU 上内存不足，但在 `Inferentia2` 上每秒可处理 248 个 token。

你可以使用 [`AWS Neuron SDK`](https://github.com/aws-neuron/aws-neuron-sdk) 探索此工作流程。主要的收获是，将模型投入运营的一个关键方面是寻找能够提高成本和能源效率的软件和硬件解决方案，如 [图 5-17](#ch05.xhtml#Figure-5-30-aws-ml-stack) 所示。最终 AWS 在 MLOps 中具有最深的协同效应。许多较小的参与者需要帮助来保持与硬件、软件和平台的这种协同效应，这一点可以从竞争解决方案的 GitHub 星数低、市场份额常见，甚至 MLOps 领域的初创公司出现负面退出等情况看出。

![](assets/5-30-aws-ml-stack.png)

# CI/CD 和开发流程自动化

MLOps 中的一个重要考虑因素是为项目设定意图。我们在做机器学习时正在努力实现什么目标？工具链会根据目标的定义而有所不同。如果目标是从一开始就实现自动化，这是 MLOPs 的核心目标，那么工具链需要支持自动化，即 `SDK` 与笔记本，如 [图 5-18](#ch05.xhtml#Figure-5.2-problem-first) 所示。

![](assets/5.2-problem-first.png)

以笔记本为先的方法对 MLOps 来说是有问题的，因为它未能通过上下文测试，即如果上下文是软件自动化，那么该目标就需要一个持续集成管道。由于笔记本本质上是“只写”且无法测试，因此目标不能与自动化相同。相反，笔记本旨在以只写、不可测试的方式探索涉及数据科学的想法。

`CI/CD`（持续集成）/（持续交付）的三个关键方面是自动化、持续反馈和测试，以及协作。让我们简要讨论一下每个方面：

*   自动化：`CI/CD` 完全依赖于自动化，以确保快速、高效且无错误的软件交付管道。这意味着软件开发管道的每个步骤都需要自动化，包括开发环境、构建软件、测试、打包和通过 `IAC`（基础设施即代码）部署。自动化还减少了人为错误的机会并确保结果一致。

*   持续反馈和测试：这里的关键收获是，每当对软件进行更改时，软件质量控制工具，包括 linting、格式化、编译和测试，都会发生。代码在每次更改时都会持续改进，因为这些质量工具会报告更改的状态。

*   协作：代码库的持续改进和能够启动新环境的能力使团队成员之间能够进行深度协作。

对此工作流程的另一个变化是 [图 5-19](#ch05.xhtml#Figure-5-7-ai-in-the-ci-cd-loop) 中显示的新技术。相对较新的是右侧红色部分中的项目，即 `IAC`、AI 编码建议和可重复的开发环境。`CI/CD` 的全部目的就是促进自动化、反馈、测试和协作。提供帮助的输入越多越好。如果你没有编译器，会对你造成伤害；如果你没有 linter 甚至没有快速 linter，也会伤害你，等等（有关此内容的更多信息，请参阅 rust 附录）。

![](assets/5-7-ai-in-the-ci-cd-loop.png)

这里的收获是，使用高性能工具是改进 `CI/CD` 管道的一种方式需要注意的地方。此外，请注意笔记本在 `CI/CD` 管道中不起作用。它怎么能工作呢？笔记本的上下文与自动化不同。虽然是一个有用的工具，但它与 MLOps 正交，因为笔记本是一种创作媒体，如新书形式。

那么 MLOps 的 `CI/CD` 管道可能是什么样子？一个好的起点是 [GitHub MLOps 模板仓库](https://github.com/nogibjj/mlops-template)。必要的项目包括可配置的基于云的开发环境，如 `GitHub`，一个在本地和构建系统中编排工作的 `Makefile`，最后是一个基于云的构建系统。

但这只是基础，类似于软件工程的基础。MLOps 的独特 `CI/CD` 组件至少包括一个模型。让我们接下来深入研究一个端到端的预训练模型示例。

## 端到端 PyTorch 到 AWS App Runner

消除理解 MLOps 中 `CI/CD` 的一些复杂性的一种好方法是构建一个 [简单示例微服务](https://github.com/nogibjj/pytorch-fastapi-aws-apprunner)，它使用预训练的 `PyTorch` 模型。为此需要修改使用 `Flask` 与 `PyTorch` 的 [官方教程](https://pytorch.org/tutorials/intermediate/flask_rest_api_tutorial.xhtml)。

首先，有一个 [`app.py`](https://raw.githubusercontent.com/nogibjj/pytorch-fastapi-aws-apprunner/main/app.py) 构建了一个用于预测的 `PyTorch` 模型，通过 `FastAPI` 提供服务。第一个函数转换图像，然后 `get_prediction` 获取推理，最后 `async def predict` 将所有内容整合在一起。

\[\[预训练 PyTorch 微服务\]\] .`FastAPI` 应用程序 `PyTorch`
