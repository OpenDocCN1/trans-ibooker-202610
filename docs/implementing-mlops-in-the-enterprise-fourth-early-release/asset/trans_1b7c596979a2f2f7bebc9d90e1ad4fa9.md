
Jupyter notebook 倾向于变得杂乱且维护不善的代码，并且需要大量手动清理工作。Linea 可以检测执行顺序错误或单元格删除，这通常会使笔记本变得杂乱且难以阅读和复现。Linea 在开发 Jupyter notebook 环境中提供的代码捕获功能可用于理解所需的工作流程并生成干净的最小可复现代码集，然后可以将该代码带入并用于其余的 [*https://github.com/LineaLabs/lineapy#use-case-1-cleaning-messy-notebooks*](https://github.com/LineaLabs/lineapy#use-case-1-cleaning-messy-notebooks) \[基于云的开发环境\]。

Linea 存在于弥合笔记本与云端开发或生产环境中所有工具之间的差距的领域，提供令人兴奋且具有巨大潜力的新功能。Linea 解决了数据从业者在尝试遵循良好的 MLOps 实践时面临的许多最具挑战性的问题。在此过程中，任何希望建立并灌输坚实的以生产为先的 MLOps 文化的组织都可以找到具有巨大潜力将组织提升到下一个水平的特性和技术。

## 摘要
本章的精神在于强调云端开发环境如何实现对生产的更接近逼近。令人反直觉的是，学术工具和工作流程在实现运营化时可能需要大量工作。GitHub Codespaces 的容器化开发环境展示了一种新兴的开发风格，带来了新的生产力提升，包括使用 AI 结对编程工具以及预训练模型。

来自 OpenAI、Hugging Face 和其他机构的预训练模型会流入用于开发 AI 软件的工具中，例如 Visual Studio Code。从本质上讲，存在一个递归反馈循环，其中 AI 工具和 AI 预训练模型的最新进展能够带来更好的 AI 解决方案。本章演示了如何在你的工作中使用这些令人兴奋的工作流程。浏览以下练习和批判性思维挑战，以将你所学的内容提升到下一个水平。

本章还为许多数据科学问题奠定了基础。一种能够解决性能、可复现性、管理性和安全性方面的解决方案是 Rust 语言。在下一章中，Rust for MLOps 被提出为 MLOps 的一种潜在的新兴解决方案。

## 练习
* 使用在 GitHub Codespaces 中开发的本地下载并版本化的模型构建 Hugging Face 命令行工具。你可以参考这个 [用于想法的示例 GitHub 项目](https://github.com/nogibjj/Coursera-MLOps-C2-Final-HuggingFace)。
* 查看此 [MLOps 模板](https://github.com/nogibjj/mlops-template) GitHub 仓库中的配置。Fork 它并制作你自己的自定义 GitHub 模板，以部署使用 GPU 和预训练模型并部署到云环境的容器化微服务。
* 使用 [此示例演练](https://codelabs.developers.google.com/tfrecommenders-flutter#0) 构建 Tensorflow 推荐系统。
* 使用此 [作为起点的示例](https://github.com/nogibjj/mlops-template/blob/GPU/hugging-face/zero_shot_classification.py) 构建一个执行零样本分类的命令行工具。
* 使用 [作为起点的 MLOps 模板](https://github.com/nogibjj/mlops-template) 运行像 MLRun 或 Bento 这样的基于 Kubernetes 的实验跟踪服务。

## 批判性思维讨论问题
使用以下问题进一步思考本章讨论的话题。
* 为什么面向 MLOps 的基于笔记本的工作流程与面向数据科学的工作流程相比会产生新的挑战？
* 为什么在 MLOPs 工作流程中使用尽可能少的工具可能是有帮助的，例如仅使用标准库工具 [`pip`](https://pip.pypa.io/en/stable/) 和 [`venv`](https://docs.python.org/3/library/venv.xhtml) 以及 [`Docker`](https://www.docker.com/)？
* AI 结对编程工作流程如何增强 MLOPs 的生产力生命周期？
* DevOps 的核心原则是什么，以及如何测试它们以确保它们已成功实施，作为进行 MLOps 的前提条件？
* 为什么在使用如下命令的容器中使用 GPU 可能具有显著优势：`docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \ python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"`

^([1](#ch09.xhtml#idm46247689795440-marker)) 近期 [在《纽约时报》报道的示例](https://www.nytimes.com/2022/08/21/technology/google-surveillance-toddler-photo.xhtml) 解释了 AI 系统可能产生具有毁灭性后果的误报。

^([2](#ch09.xhtml#idm46247689750256-marker)) 在播客 [“未来有多重要"'](https://www.samharris.org/podcasts/making-sense-episodes/292-how-much-does-the-future-matter) 中，William MacAskill 提出，大多数高级机器学习研究人员认为“未来二十年内 AI 在你一生中导致死亡或剥夺权力的几率为 2.5%。这一统计数据比你在车祸中死亡的几率更显著。”一个关键问题是推荐引擎是否属于此类容易导致灾难性风险的 AI

# 第10章 使用 Rust 实现 MLOps
运营效率必须是任何技术系统的核心。MLOps 建立在 DevOps 之上，而 DevOps 建立在 *kaizen* 的概念之上，*kaizen* 是指持续改进的日语词汇。没有持续改进，就不会有 DevOps，也就不会有 MLOps。

持续改进运营的核心是一个简单的问题：“我们能否将运营性能——从训练和推理到打包和交付——提高十倍或更多？”如果答案是肯定的，就像许多使用 Python 进行数据科学的组织一样，下一个问题应该是：“我们为什么不这样做？”

几十年来，组织在机器学习解决方案方面除了纯 C 或 C++ 和 Python 外几乎没有其他选择。C++ 在性能方面可能提供更高的效率，但 Python 通常更容易学习、实现和维护，这就是为什么 Python 在数据科学中蓬勃发展。性能高但复杂的 C++ 与易于学习但相对较慢的 Python 之间的艰难选择最终导致许多公司选择 Python。

但还有另一种方式。Rust 一直位列 [性能最高且节能效率最高的语言](https://greenlab.di.uminho.pt/wp-content/uploads/2017/10/sleFinal.pdf) 之一。它也是 [StackOverflow 年度开发者调查](https://survey.stackoverflow.co/2022/) 中最受喜爱的语言之一。尽管数据科学中广泛使用的一些 Python 库是用 C 编写的，可以提供运行编译语言的一些性能优势，但 Rust 在使用单一语言的同时提供了更直接通往裸机的途径。

Rust 也比 C 或 C++ 更容易学习和使用，这使其成为想要编译语言性能的人的现实解决方案。当使用 [GitHub Copilot](https://github.com/features/copilot) 时尤其如此。这个 AI 驱动的结对编程助手使用 OpenAI Codex 在开发者编码时实时为其建议代码和整个函数。让我们接下来讨论这一策略。

### 注
短语 *AI 驱动的结对编程助手* 指的是对经典结对编程风格的改进，在经典风格中你坐在另一位开发者旁边一起编写代码。随着 [GitHub Copilot X](https://github.blog/2023-03-22-github-copilot-x-the-ai-powered-developer-experience/) 等新兴开发者工具的出现，你可以与编码助手聊天以获取有关编码项目的想法，同时在你输入时获得编码建议。

## Rust 用于 MLOps 的案例
