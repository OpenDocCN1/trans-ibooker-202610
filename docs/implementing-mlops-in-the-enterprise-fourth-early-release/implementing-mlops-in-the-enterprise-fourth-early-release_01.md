## 前言

在现代商业世界的不断演变中，适应并利用新兴技术的重要性怎么强调都不为过。本书深入探讨前沿发展及其对企业运营、竞争和蓬勃发展方式的变革性影响。从 ONNX 和 Rust 到 Mojo、大语言模型（LLMs）、ChatGPT 和 Hugging Face，本书为读者提供了对最新技术进步及其在企业生态系统中的实际应用的全面理解。

第一章通过探讨新兴技术、方法及其对企业的意义来奠定基础。后续章节深入探讨 ONNX（开放神经网络交换）如何彻底改变了 AI 模型在各平台间的共享和部署方式。与此同时，Rust 这种系统编程语言因其性能、可靠性和安全性而备受关注。它还涵盖了像 Mojo 这样的新语言以及 OpenAI 的 ChatGPT 和 Hugging Face 的 StarCoder 等大语言模型日益增长的影响力。这些技术改变了自然语言处理，使用户与机器之间的交互更具人性化。章节最后分析了这些技术的竞争优势以及企业如何在快速变化的全球市场中利用它们取得成功。

最后，附录中采访了许多前沿 AI 和 ML 领域的顶尖人物，并提供了资源帮助读者快速掌握 ML 认证和 Rust 语言。

## 本书适合谁阅读

本书面向渴望深入 MLOps 的企业开发人员和数据科学家，重点关注 ONNX、Rust、Mojo、Kubernetes LLMs、ChatGPT 和 Hugging Face 等新兴技术。本书旨在介绍超越传统 Jupyter、Conda、Pandas、Numpy 和 Sklearn 堆栈的替代 MLOps 工作流程。它提供了一条发现更高效、更环保、更具开创性解决方案的途径。

本书的驱动力是希望在不依赖传统 Jupyter、Conda、Pandas、Numpy 和 Sklearn 堆栈的情况下探索 MLOps 和数据科学的可能性。我们旨在发现强调性能、简洁性、计算速度和生态友好性的解决方案。鉴于 Rust 作为最受欢迎和最受追捧的语言之一的不断增长的流行度，我们将研究其在 MLOps 领域和其他新兴技术中的潜力。

## 本书组织结构

本书涵盖了 MLOps 的各个方面，重点关注 ONNX、Rust、Kubernetes、LLMs、ChatGPT 和 Hugging Face 等新兴技术。各章节结构如下：第 1 章，“MLOps - 它是什么，为什么我们需要它？”，介绍了 MLOps、其重要性以及它与 DevOps 的区别。第 2 章，“MLOps 的阶段”，深入探讨 MLOps 的各个阶段，详细说明 MLOps 项目中涉及的必要组件和流程。第 3 章，“开始你的第一个 MLOps 项目”，指导你开始第一个 MLOps 项目，提供规划和论证 MLOps 项目的框架。第 4 章，“使用数据和特征存储”，涵盖使用数据和特征存储，讨论数据访问、版本控制和特征存储摄取。第 5 章，“为生产开发模型”，重点介绍为生产开发模型，包括超参数调优、AutoML 和大规模训练。第 6 章，“模型和 AI 应用的部署”，涉及模型和 AI 应用的部署，探讨模型注册表、管理和实时服务管道。第 7 章提供了从头到尾构建生产级 MLOps 项目的分步指南，并附有真实代码示例。第 7 章，“构建可扩展的深度学习和大语言模型项目”，深入探讨构建可扩展的深度学习项目，讨论存储基础、数据标注和部署策略。第 8 章，“高级数据类型解决方案”，探讨时间序列、NLP、视频分析和图像分类等高级数据类型的解决方案。第 9 章，“常见用例解决方案”，提供欺诈预测、预测性维护和实时推荐引擎等常见用例的解决方案。第 10 章，“使用 Rust 实施 MLOps”，研究 Rust 在 MLOps 中的作用、其优势及其在该领域的潜在应用。每章都包括批判性思维问题和练习，非常适合团队讨论、用户组、求职面试准备或认证。练习有助于通过实践学习，将章节内容应用到可作为优秀作品集的工作代码示例中。

## 其他资源



对于拥有 O’Reilly 平台访问权限的人来说，一个可能有助于你 AWS 之旅的可选资源是 [52 周 AWS 完整系列](https://oreil.ly/kDf0Q)；它包含对所有重要认证的数小时讲解。本系列还可在所有 [主要播客频道](https://podcast.paiml.com) 上免费收听播客。

此外，除了这些资源之外，Noah Gift 在以下位置提供关于 AWS 的每日和每周更新：

### Noah Gift O’Reilly
在 [Noah Gift 的 O’Reilly 个人资料](https://www.oreilly.com/pub/au/3039) 上，有数百个视频和课程涵盖各种技术，包括 AWS，以及频繁的现场培训。另有两本涵盖 AWS 的 O’Reilly 书籍也可能有所帮助：[*面向 DevOps 的 Python*](https://www.oreilly.com/library/view/python-for-devops/9781492057680) 和 [*实用 MLOps*](https://www.oreilly.com/library/view/practical-mlops/9781098103002)。

### Noah Gift LinkedIn
在 [Noah Gift 的 LinkedIn](https://www.linkedin.com/in/noahgift) 上，他定期直播 AWS 培训以及 O’Reilly 书籍的进行中笔记。

### Noah Gift Website
[*Noahgift.com*](https://noahgift.com) 是查找新课程、文章和演讲最新更新的最佳地点。

### Noah Gift GitHub
在 [Noah Gift 的 GitHub 个人资料](https://github.com/noahgift) 上，你可以找到数百个仓库和几乎每日的更新。

### Noah Gift AWS Hero Profile
在 [Noah Gift 的 AWS Hero 个人资料](https://oreil.ly/FyFvf) 上，你可以找到涉及 AWS 平台工作的当前资源的链接。

### Noah Gift Coursera Profile
在 [Noah Gift 的 Coursera 个人资料](https://www.coursera.org/instructor/noahgift) 上，你可以找到多个专业课程，涵盖云计算的广泛主题，重点放在 MLOPs、数据工程和 MLOps 上。这些课程包括：
* [DevOps、DataOps、MLOps](https://www.coursera.org/learn/devops-dataops-mlops-duke)
* [MLOps 平台：Amazon SageMaker 和 Azure ML](https://www.coursera.org/learn/mlops-aws-azure-duke)
* [MLOps 的开源平台](https://www.coursera.org/learn/open-source-mlops-platforms-duke)
* [面向 MLOps 的 Python 基础](https://www.coursera.org/learn/python-essentials-mlops-duke)
* [云计算基础](https://oreil.ly/XcxeC)
* [云虚拟化、容器和 API](https://oreil.ly/mcacu)
* [云数据工程](https://oreil.ly/pVHGm)
* [云机器学习工程和 MLOps](https://oreil.ly/724Sz)
* [数据工程的 Linux 和 Bash](https://oreil.ly/Ps1Mz)
* [数据工程的 Python 和 Pandas](https://oreil.ly/07ywX)
* [数据工程的 Python 和 SQL 脚本编写](https://oreil.ly/rSBMG)
* [数据工程的 Web 应用和命令行工具](https://oreil.ly/gDDHy)
* [大规模构建云计算解决方案专业课程](https://oreil.ly/VZo7x)
* [数据工程的 Python、Bash 和 SQL 基础专业课程](https://oreil.ly/TpIUU)

### Pragmatic AI Labs Website
许多与 AWS 相关的免费资源可在 [*Pragmatic AI Labs and Solutions 网站*](https://paiml.com) 获取。这些包括几本免费书籍：
* [*数据分析的云计算*](https://oreil.ly/I4ADa)
* [*精简 Python*](https://oreil.ly/JD2Ha)
* [*Python 命令行工具：使用 Click 设计强大的应用*](https://oreil.ly/7zxNa)
* [*Python 中的测试*](https://oreil.ly/uJ5cC)

# 本书中使用的约定
本书中使用了以下排版约定：
*斜体*
: 
表示新术语、URL、电子邮件地址、文件名和文件扩展名。
`Constant width`
: 
用于程序列表，以及在段落中引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。
**等宽粗体**
: 
显示用户应逐字输入的命令或其他文本。
`Constant width italic`
: 
显示应由用户提供的值或由上下文确定的值替换的文本。

## 提示
此元素表示提示或建议。

### 注释
此元素表示一般说明。

### 警告
此元素表示警告或注意。

# 使用代码示例
PROD: 请联系作者确认他们是否会将代码示例上传到 oreilly.com 或其自己的网站（例如 GitHub）。如果没有代码下载，请删除整个章节。如果有，在向 digidist 发送电子邮件并附上链接时，请告知他们 `title_title` 的填写内容（应尽可能接近书名，例如 `learning_python_2e`）。此信息将决定 digidist 加载文件的位置。

补充材料（代码示例、练习等）可在 [*https://github.com/nogibjj/rust-mlops-template*](https://github.com/nogibjj/rust-mlops-template) 下载。

如果您在使用代码示例时有技术问题或遇到问题，请发送电子邮件至 *bookquestions@oreilly.com*。

本书旨在帮助您完成工作。一般来说，如果本书提供了示例代码，您可以在自己的程序和文档中使用它。除非您复制代码的很大一部分，否则无需联系我们征求许可。例如，使用本书中几段代码编写程序不需要许可。出售或分发 O’Reilly 书籍中的示例需要许可。引用本书并引用示例代码来回答问题不需要许可。将大量示例代码纳入产品文档中需要许可。

我们感谢但通常不需要署名。署名通常包括标题、作者、出版商和 ISBN。例如：

如果您认为代码示例的使用超出了合理使用范围或上述许可，请随时联系我们 *permissions@oreilly.com*。

# O’Reilly 在线学习
## 注释
40 多年来，[*O’Reilly Media*](https://oreilly.com) 一直提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业技能。O’Reilly 的在线学习平台为您提供按需访问现场培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频集合。有关更多信息，请访问 [*https://oreilly.com*](https://oreilly.com)。

# 如何联系我们
请将有关本书的评论和问题发送给出版商：
* O’Reilly Media, Inc.
* 1005 Gravenstein Highway North
* Sebastopol, CA 95472
* 800-998-9938（美国或加拿大）
* 707-829-0515（国际或本地）
* 707-829-0104（传真）

请不要忘记更新上面的链接。

发送电子邮件至 *bookquestions@oreilly.com* 对本书发表评论或询问技术问题。

有关我们书籍和课程的新闻和信息，请访问 [*https://oreilly.com*](https://oreilly.com)。

在 LinkedIn 上找到我们：[*https://linkedin.com/company/oreilly-media*](https://linkedin.com/company/oreilly-media)。

在 Twitter 上关注我们：[*https://twitter.com/oreillymedia*](https://twitter.com/oreillymedia)。

在 YouTube 上观看我们：[*https://youtube.com/oreillymedia*](https://youtube.com/oreillymedia)。

# 致谢
## Noah
能够参与 O’Reilly 书籍的编写始终是一种荣幸。本书是我第五本 O’Reilly 出版的书籍，很可能也是我最后一本技术书籍，因为我正转向其他写作和内容创作形式。感谢我在 O’Reilly 的所有合作者，包括现任和前任编辑以及合作者和近期书籍的作者。



Also, thanks to many of my current and former students, faculty, and staff at [杜克 MIDS](https://oreil.ly/2SWqF), [杜克人工智能工程硕士](https://oreil.ly/bTcbr), as many ideas in this book came from courses I taught, and questions brought up from students.

Finally, thank you to my family, Leah, Liam, and Theodore, who put up with me working on weekends and late at night to hit deadlines.
