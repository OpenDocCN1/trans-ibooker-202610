## Preface

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the Preface of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at rfernando@oreilly.com.

Cloud computing has been widely adopted as a model of next-generation digital business transformation that drives growth and innovation. Nowadays, customers look for an ecosystem and experience that is fast and that seamlessly integrates with their existing services. From an enterprise perspective, the cloud delivers services for consumers and businesses in a way that is scalable, highly reliable, and available. From an end-user perspective, the cloud provides a simple model for acquiring computing services without needing to fully understand the underlying infrastructure and technology.

To take full advantage of the speed and agility of cloud services, many existing applications have been transformed into cloud native applications, and new solutions are being built for the cloud first. Cloud native applications are built from the ground up to embrace rapid change, large scale, and resilience. By default, the underlying infrastructure for cloud native applications plays a critical role in efficiently serving a business’s needs. If the underlying infrastructure is not architected with the correct practices, even the best cloud native applications will fail in production environments.

This book explores how modern cloud native infrastructures on Azure can be built and managed in a production environment, along with various requirements and design considerations of cloud native applications.

### Who Should Read This Book

This book provides a simple yet comprehensive introduction to the cloud native landscape and all the major technologies that engineers use to build such reliable environments. The book is for site reliability engineers (SREs), DevOps engineers, Solution architects, Azure enthusiasts, and anyone who is involved in building, migrating, deploying, and managing the day-to-day operations of a cloud native workload.

The book assumes that you possess basic knowledge of the cloud and DevOps culture in general. But even if you don’t, and you want to better understand the buzz around cloud native and other fancy technologies, this book is still the right place for you to get started.

### Goals of This Book

By the end of this book, you will be able to follow and build your own infrastructure on Microsoft Azure. We present a sequential introduction to the major components of the cloud native world and how to use, deploy, and maintain them over Azure. Additionally, you will be introduced to the need for cloud native technologies in today’s brave new world, the problems they solve, and hands-on best practices.

In this book, you will:

- Learn how to build cloud native infrastructure over Azure following the path of the Cloud Native Computing Foundation’s (CNCF) cloud native landscape
- Ascertain which technologies to use at different stages of design
- Discover how to address the problems you may face while managing and operating cloud native infrastructure, as well as the technologies that help solve them

### Navigating This Book

This book is organized as follows:

- Chapter 1 provides a basic introduction to the cloud and the need for cloud native technology and its adaptations.
- Chapter 2 covers the fundamentals of infrastructure as code (IaC) with Terraform and Packer, and introduces Azure and Ansible as provisioners/configuration managers.
- Chapter 3 introduces containers, and container runtimes such as containerd, Docker, and CRI-O. Different types of container registries are also discussed.
- Chapter 4 discusses Kubernetes and includes the details necessary to use the infrastructure in upcoming chapters.
- Chapter 5 deals specifically with the Azure Kubernetes Service and the Helm package manager.
- Chapter 6 focuses on how modern cloud native infrastructure can be made observable.
- Chapter 7 is all about service discovery and service mesh. We introduce the CoreDNS DNS server and the Istio service mesh in this chapter.
- Chapter 8 covers network and policy management, including the Calico, Flannel, Cilium, Azure Policy, and Open Policy Agent container networking interfaces.
- Chapter 9 discusses how persistent storage systems are deployed on cloud native infrastructure, with a focus on Azure Storage, Vitess, Rook, TiKV, and etcd.
- Chapter 10 is primarily focused on messaging and streaming platforms such as NATS and Azure messaging services.
- Chapter 11 is a simple introduction to the serverless side of things in the cloud native landscape.
- Chapter 12 serves as a wrap-up of everything we discussed in the preceding chapters.

### Acknowledgments

We would like to thank Rita Fernando, Nicole Taché, and Jennifer Pollock, our editors at O’Reilly who were supportive and awesome as they guided us through the writing process. We would also like to thank our tech reviewers for their valuable contributions to the book. This book would not have been possible without the thorough and amazing reviews and feedback from Matt Franz, Peter Jausovec, Liudmila Sinkevich, Steve Machacz, and Alexander Kabonov.

In addition, we would like to highlight Matt Franz for his thorough reviews, suggestions, and contributions to improve the quality of the book. Matt gave extensive feedback on chapters and contributed Chapter 10.

Nishant would like to thank his mom, his dad, and his wife, Mahek, for being so patient and supportive while he was working on the book. He would also like to thank the O’Reilly team and his peers at LinkedIn who supported the idea from the very beginning.

Michael would like to thank his family, who encouraged him to follow his dreams and never give up. He would also like to thank his girlfriend, Jennifer, for her support and patience while he was working on this book, the O’Reilly staff for helping to bring the book from idea to reality, and all of the reviewers for their time.





## 本书使用的约定

本书使用了以下排版约定：

*Italic*

表示新术语、URL、电子邮件地址、文件名和文件扩展名。

*Constant width*

用于程序列表，以及在段落中引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。

*Constant width bold*

显示用户应逐字键入的命令或其他文本。

*Constant width italic*

显示应由用户提供的值或由上下文确定的值替换的文本。

TIP

此元素表示提示或建议。

NOTE

此元素表示一般说明。

WARNING

此元素表示警告或注意事项。

## 使用代码示例

补充材料（代码示例、练习等）可从 https://github.com/oreillymedia/title_title 下载。

如果您对代码示例有技术问题或在使用时遇到问题，请发送电子邮件至 bookquestions@oreilly.com。

本书旨在帮助您完成工作。一般来说，如果本书提供了示例代码，您可以在自己的程序和文档中使用它。除非您要复制代码的很大一部分，否则无需联系我们征求许可。例如，使用本书中多个代码片段编写程序不需要许可。出售或分发 O’Reilly 书籍中的示例需要许可。引用本书并引用示例代码来回答问题不需要许可。将大量示例代码纳入您产品的文档中需要许可。

我们感谢，但通常不需要署名。署名通常包括标题、作者、出版商和 ISBN。例如：“Cloud Native Infrastructure with Azure by Nishant Singh and Michael Kehoe (O’Reilly). Copyright 2022 Nishant Singh and Michael Kehoe, 978-1-492-09096-0.”

如果您认为代码示例的使用超出了合理使用范围或上述许可，请随时联系我们 permissions@oreilly.com。

## O’Reilly Online Learning

NOTE

40 多年来，O’Reilly Media 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业知识。
O’Reilly 的在线学习平台为您提供按需访问实时培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频收藏。欲了解更多信息，请访问 http://oreilly.com。

## 如何联系我们

请将有关本书的评论和问题发送给出版商：

O’Reilly Media, Inc.

1005 Gravenstein Highway North

Sebastopol, CA 95472

800-998-9938（美国或加拿大境内）

707-829-0515（国际或本地）

707-829-0104（传真）

我们为本书设有网页，在该页面上列出勘误、示例和任何其他信息。您可以在 https://oreil.ly/cloud-native-azure 访问此页面。

发送电子邮件至 bookquestions@oreilly.com 对本书发表评论或提出技术问题。

有关我们的书籍和课程的新闻和信息，请访问 http://oreilly.com。

在 Facebook 上找到我们：http://facebook.com/oreilly

在 Twitter 上关注我们：http://twitter.com/oreillymedia

在 YouTube 上观看我们：http://youtube.com/oreillymedia

## 作者简介

Nishant Singh 是 LinkedIn 的高级站点可靠性工程师，他致力于提高网站的可靠性，重点是减少检测事件的平均时间（MTTD）和响应事件的平均时间（MTTR）。在加入 LinkedIn 之前，他曾在 Paytm 和 Gemalto 担任 DevOps 工程师，花时间为客户构建定制解决方案，并在公有云上管理和维护服务。Nishant 对站点可靠性工程和构建分布式系统有浓厚兴趣。

Michael Kehoe 是 Confluent 的高级安全工程师。此前，他在 LinkedIn 担任高级站点可靠性工程师，负责事件响应、灾难恢复、可视化工程和可靠性原则。在 LinkedIn 任职期间，他领导了公司自动化迁移到 Microsoft Azure 的工作。Michael 专注于维护大型系统基础设施，这一点在他于 LinkedIn（应用程序、自动化和基础设施）和昆士兰大学（网络）的工作中得到了体现。他还曾在 NASA 建造小型卫星，并在 Rio Tinto 编写热环境软件。
