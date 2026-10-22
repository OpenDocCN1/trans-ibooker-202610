## Preface

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the Preface of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at aking.em@gmail.com.

The Internet of Things (IoT) is a complex, interconnected system-of-systems composed of different hardware devices, software applications, data standards, communications paradigms, and cloud services. Knowing where to start with your own IoT project can be a bit daunting. *Programming the Internet of Things* is designed to get you started on your IoT development journey, and shows you, the developer, how to make the IoT work.

If you decide to stay with me through this book, you’ll learn how to write, test and deploy the software needed to build your own basic, end-to-end IoT capability.

## Who Is This Book For?

*Programming the Internet of Things* is written for programmers by a programmer. It’s also designed for any technologist (programmer or not) interested in learning about, and building, end-to-end IoT solutions, as well as college-level instructors teaching their own connected devices course.

This is not designed as a reference book, nor does it provide an exhaustive deep dive into and analysis of the various protocols commonly used for communicating amongst IoT devices and cloud services. There are extremely well-written specifications for this purpose (although we’ll certainly dive into some of these details to better understand these protocols).

Those who are interested in practicing what is learned in each chapter can move straight into the exercises at the end of each chapter, while those interested more in the concepts and general knowledge can simply move on to the next section. I’ve organized this book so that each of these three groups - Programmers, Instructors, and Information Technology (IT) Executives - can benefit.

## To the Programmer

If you’re embarking on your own IoT learning journey as a practitioner, I’m assuming you’re mostly interested in expanding your skillset, as you’ve witnessed the growth of IoT opportunities and want to be part of this important technology evolution. Over the years, I found the step-by-step, “build from

the ground up" approach to implementing integrated and open IoT solutions to be most helpful in understanding the complexities of this area, and so I follow this model throughout the book.

The programming examples you’ll encounter throughout this book are constructed from my own journey in learning about the IoT, and have evolved to represent many of the lab module assignments from the graduate-level Connected Devices course I teach as part of Northeastern University’s Cyber Physical Systems program.

Each chapter and exercise builds upon the previous, so if you’re starting out with the IoT, I’d recommend walking through each as-is, in the order given. If you’re experienced in this space and are using this book as a reference guide, you may consider incorporating specific lessons of interest into your own application.

If you’re new to the IoT and unfamiliar with how an end-to-end IoT system comes together through software, I’d recommend walking through the book in order, working through each exercise, and consider application customizations only after you’ve mastered each chapter.

## To the Instructor

The contents that form the underpinnings of this book have been used successfully in my graduate-level Connected




过去三年中设备课程的讲义。这本书是对这些讲义、演示文稿、示例和实验练习的正式化，其结构与我的课程基本相同。

最初的课程设计为物联网入门，但随着学生反馈和我的助教建议，它很快演变为一门高级、以项目为导向的软件开发课程，作为东北大学网络物理系统系硕士生毕业前必修课之一。该课程的目标是建立坚实的物联网知识基础，帮助学生从学术界转向工业界，将基础的物联网技能和知识带入各自的组织。

本书的结构使其可作为参考指南使用，甚至可作为完整课程的重要组成部分，帮助您开展自己的物联网相关课程。内容侧重于构建端到端、开放且集成的物联网解决方案，从设备到云，采用“边建边学”的教学方法。由于每一章都建立在前一章的基础上，您可以使用本书指导学生从零开始构建自己的平台。

您可以在本书网站（https://programmingtheiot.com/programming-the-iot-book/）上了解练习和其他有助于教学和解释概念的相关内容，保持最新。

## 致技术经理和主管

本书的内容应有助于您更好地理解任何物联网项目固有的集成挑战，并提供有关帮助您的技术团队在物联网计划中取得成功的技能要求的见解。

如果您属于这一群体，我的假设是您主要关心的是整体理解这一技术领域——其集成挑战、开发团队设置要求和需求、团队技能组合、围绕物联网的业务用户/利益相关者关注点，以及您在组织踏上物联网之旅时可能遇到的潜在变革管理挑战。

## 我需要知道什么？

尽管本书中的练习假设您具有编程经验，但大多数练习不需要复杂的编程技能或正式的计算机科学背景。然而，如果您打算完成大多数章节末尾的练习，您需要具备在 Python 和 Java 编程语言上构建简单应用程序的基本熟练程度；在一个集成开发环境（IDE）中工作；阅读、编写和执行单元测试；以及通过 shell 命令行配置基于 Linux 的系统。

所有练习都以特定任务的待设计图为先导，该图提供了关于您构建的任何新逻辑组件如何与现有组件协同工作的具体细节，以及您已开发的现有组件。大部分这些图表将遵循统一建模语言（UML）¹ 指定的准则，并结合其他非 UML 设计构造，以便使设计尽可能清晰。图例在被认为能增加足够价值时包含在内，在认为只是增加噪音时则排除。

对于技术高管和经理，您无需亲自实施练习，但通读整本书将有助于您了解团队可能遇到的挑战。

对于主要想了解物联网涵盖内容的业务利益相关者，我建议至少阅读每章的概述部分，然后将重点放在最后一章，该章讨论了一些实际案例、场景和实施建议。

## 本书如何安排？

本书将带您逐步使用各种开源库和软件组件构建端到端、全栈且集成的物联网解决方案。您将构建的每个组件都将成为更大系统的一部分，并且在完成每章练习时，您将看到它们如何相互连接并映射到最终状态架构。

如果您不打算实施练习，只想初步了解各种物联网技术，请随意跳到对您最感兴趣且最相关的部分。

我将相似的章节归为一组，并采用此方案确定本书的四个部分：第一部分 - 入门，第二部分 - 连接物理世界，第三部分 - 连接其他设备，第四部分 - 连接云。让我们更详细地回顾一下每一部分。

## 第一部分 – 入门

在本节中，我们为物联网开发建立初始基础。您将首先创建开发和测试环境，并在本节结束时编写两个简单的应用程序，以验证我们的环境是否正常工作。

- 第 1 章是本书中最长的章节，因为它为构建端到端解决方案奠定了基础。它将帮助您建立物联网知识的基础并设置工作站和开发环境，以便您能尽快提高生产力。我将介绍一些基本的物联网术语，创建一个简单的问题陈述，定义核心架构概念，并建立一个初始设计方法，我将以此作为后续每个练习的框架。

#### NOTE

物联网由大量异构设备和系统组成，有许多工具和实用程序可用于支持开发和系统自动化。本书中我使用的工具和实用程序属于开源类别，仅代表您可用的众多工具中的一小部分。当然，这些不应被视为 carte blanche 推荐，因为您可能有自己偏好且最适合特定需求的工具。我的目标只是让内容保持良好界限，并帮助传达一种通用的开发和自动化方法，帮助您成功实施本书中的练习。

- 第 2 章涵盖您的开发环境设置和需求捕获方法，然后进入编码。在本章中，您将创建前两个物联网应用程序——一个用 Python，另一个用 Java。这些会非常简单，但为后续章节奠定了基础。即使您是经验丰富的开发人员，也应按给定方式完成练习，以便我们今后从同一基准开始工作。

## 第二部分 – 连接物理世界

本节涵盖物联网的一个基本特征——与物理世界的集成。您将学习如何读取传感器数据并触发执行器，既可通过模拟器虚拟实现，也可在实际物联网设备上实现。

- 第 3 章探讨了您可以从物理世界收集数据（传感）的方式，以及基于该数据触发操作（执行）的方式。您将首先构建一组简单的模拟器，您可能在每个后续练习中继续使用。这些简单的模拟器将帮助您模拟真实传感器并允许您触发执行事件。
- 第 4 章在第 3 章的基础上，讨论了如果您选择在一個或多个物理设备上构建和运行应用程序，如何连接少量真实传感器和执行器。本章重点关注 Raspberry Pi，并假设硬件运行 Raspbian 操作系统。
- 第 5 章讨论遥测和数据格式化。我将讨论如何结构化您的数据，以便人类和机器都易于存储、传输和理解。这将成为您与其他“事物”互操作的基础。

## 第三部分 – 连接其他事物

这就是所谓的“实战”部分。本节侧重于跨设备的集成，因为要真正集成，您需要一种方法将遥测和其他信息从一个地方传输到另一个地方。本节的章节侧重于学习和利用为物联网生态系统设计的应用层协议。我将假设您的网络层已就位并正常工作，尽管我会在途中讨论一些无线协议。

- 第 6 章介绍发布/订阅协议——具体为消息队列遥测传输（MQTT）和其他在物联网应用中常用的相关标准。我将介绍一组选定的规范，并解释您如何开始构建一个简单的抽象层，让您能够轻松地与常见的开源库接口。
- 第 7 章几乎与第 5 章相同，只是它侧重于请求/响应协议——具体为受限应用协议（CoAP）和其他在物联网应用中也常用的标准。
- 第 8 章探讨同时使用发布/订阅和请求/响应协议的情况。您将把 MQTT 和 CoAP 集成到解决方案中，并了解这两种协议如何相互补充。
- 第 9 章介绍另一种协议——OPC UA（开放平台通信统一架构），使您能够同时使用发布/订阅和请求/响应



通过单一协议标准实现通信范式。

## 第四部分 – 连接到云端

最后，在集成栈的“顶端”，你将学习如何通过使用网关应用程序作为云功能与所有设备之间的中介，将你的所有物联网设备基础设施连接到云端。

本部分涵盖基本的云连接原则，并简要介绍可以存储、分析和管理你的物联网环境的各种云服务。在每种情况下，你将在每个平台上构建相同的简单云应用程序。

- 第 10 章探讨了基于开源平台构建的各种物联网中心云服务，这些服务可以重新托管在你自己的环境中，也可以通过免费或付费层访问。
- 第 11 章概述了具有物联网特定功能的各种商业云服务。由于这些平台有许多书籍和教程可供参考，我不会详细介绍构建任何具体内容；本章将仅作为这些功能的一个回顾。
- 第 12 章考察了我发现对准备我在东北大学的《连接设备》课程特别有帮助的几个简单的物联网用例。具体来说，我将介绍家庭环境监测系统、宠物监测系统和水培花园管理系统的问题陈述、预期结果、设计方法和目标解决方案。

在每章中，我都会提供主题的简要介绍以及一些有用的背景材料，其中将包括一些有助于理解我将讨论的概念的相关定义。我还将总结为什么每个主题都很重要以及你可以期望学到什么。每章都以一些动手实践练习结束，以帮助你将所呈现的信息应用到自己的应用程序中。

我希望这种方法能让你理解并创建端到端的集成物联网系统。它也设计成你可以跳到对你最相关的章节作为参考指南，因此如果你对编写解决方案不感兴趣，可以跳过练习，尽管我建议你尝试一下！如果你决定深入每个实验模块，请记住它们是累积的，即你在每章中所做的工作通常会在后面的章节中使用或引用。

## 有关物联网的一些背景

了解是什么促成了这些生态系统可能会有所帮助，因此以下是我们如何走到这一步的简要总结。计算在 1950 年代晶体管的发明下取得了重大进展，随后在 1960 年代戈登·摩尔发表了一篇论文，描述了在同一物理空间中封装的晶体管数量翻倍（后来在 1970 年代更新）²。

随着现代计算的到来，出现了现代网络，以及互联网的开端，即 1969 年 ARPAnet 的发明³。这导致在 1970 年代使用网络控制协议（NCP）和传输控制协议（TCP）通过互联网协议（IP）以及利用现有有线基础设施对数据进行分块或分组化。这对工业很有用，使电气工业自动化能够沿着集中管理分布式、连接系统的道路发展。监控与数据采集（SCADA）系统从其专有根源中出现，可编程逻辑控制器（PLC）被开发出来以利用 TCP/IP 网络和相关设备标准⁴。

最终，我们迎来了 1980 年代，用户数据报协议（UDP）⁵的引入以及许多人体验到的早期现代互联网的诞生——万维网（WWW）——由蒂姆·伯纳斯-李在 1980 年代末发明⁶。

我相信你已经注意到一个共同主题：一个问题随后伴随着技术创新（通常具有专有性质）来解决挑战，然后变得标准化（或被一个或多个标准取代），从而导致广泛采用和进一步创新。

这就把我们带到了物联网时代，在 1980 年代末和 1990 年代初，我们开始看到第一个连接设备的出现，包括由 John Romkey 和 Simon Hackett 在 1990 年代 Interop 展会上演示的互联网连接烤面包机⁷。不久之后，一台网络摄像机于 1991 年在剑桥大学计算机实验室的特洛伊房间附近的咖啡壶上设置⁸。因为谁想去咖啡机那里却发现咖啡壶是空的呢？

当然，随后出现了更多设备，我猜甚至更多设备被构建并作为大学实验室、宿舍、家庭、公寓和企业的实验而连接。与此同时，计算和网络变得越来越便宜、强大，当然也越来越小。1999 年，Kevin Ashton 在宝洁公司发表了“物联网”⁹，被广泛认为是对该短语的首次命名。

快进到 2005 年，意大利互动设计学院为我们提供了价格低廉且专为新手设计的 Arduino 单板计算机（SBC）¹⁰，为更多人开始构建自己的传感和自动化系统打开了大门。加上易于访问的存储和通过互联网上的任何地方可达的服务来分析数据的能力，我们就有了物联网生态系统的基础；也就是说，很多不同的东西可能在个体上是独特的，但可以以这样的方式相互连接，从而服务于更大的目的。

从这个意义上说，我认为最好不要将物联网视为一堆将物理世界和互联网连接起来以执行有用工作的东西。物联网的本质是异构性：就设备类型、功能和能力、目的以及实现方法、支持的协议、安全性和管理技术而言存在大量差异。

## 复杂性重新定义

那么，“物联网”*确切地*是什么？它是一套复杂的连接物理世界到互联网的技术生态系统，使用各种*边缘计算设备*和*云计算服务*。

就本书而言，边缘计算设备代表嵌入式电子产品、小型计算机和软件应用程序，它们要么通过传感器和执行器直接与物理世界交互，要么为这些设备和应用程序提供网关或桥梁，以便连接到互联网。云计算服务代表位于一个或多个数据中心内且始终可通过互联网访问的计算系统、软件应用程序、数据存储和其他计算服务。

当然，这是对两个复杂且经常被过度使用的技术术语的过度简化。我的意图是为讨论物联网和你在本书练习中构建的集成系统提供相对简单的关键计算资源分类。

话虽如此，在本书的其余部分，我将专注于物联网在架构和目的方面的内容；也就是说，物联网系统是如何设计的，以及设计预期实现的结果。

第一步是将这两个技术术语边缘计算设备和云计算服务映射到*架构层*，其中边缘计算设备是*边缘层*的一部分，而云计算服务是*云层*的一部分。这提供了物联网系统关键功能的物理和逻辑分离，意味着例如，所有传感和执行都将在边缘层进行，而所有长期存储和复杂分析将在云层进行。

这种层的分离代表了系统架构的高级描述，即我将继续使用的计划，以描述构成集成物联网系统的各种物理和逻辑组件的组成。

#### NOTE

工业互联网联盟（IIC）已发布了关于物联网子集，即工业物联网（IIoT）的各种有用文件。其中一份出版物《工业互联网参考架构》（位于此处 https://www.iiconsortium.org/IIRA.htm）讨论了 IIoT 系统的框架和通用词汇，并极大地影响了我对物联网架构主题的思考。¹¹





任何物联网系统的真正价值在于其提供测量和分析的能力。当收集到足够的测量数据，并且具有足够的粒度与样本量时，就可以进行分析以确定系统的运行情况。这种分析提供的洞察有助于判断系统是否实现了预期目标，或者是否需要进行修正。

例如，如果我每分钟测量一次冰箱内部温度，我就能确定冰箱内储存的物品在特定温度下暴露了多长时间。如果我每天只采样一次冰箱内部温度，我就没有足够的细节来做出该判断。

当然，我收集的数据需要能被其他系统理解，否则几乎毫无用处。构建可集成的物联网解决方案需要物联网系统中每个部分的设计者仔细思考其设计（和数据）如何与其他系统交互。这就是为什么，虽然这些部分 individually 可能独特且 NOT 依赖于其他部分，但作为开发者，你需要考虑其他系统（和其他开发者）如何需要与你的软件和数据交互，以实现我们一直在讨论的这一更大目标。

在构建集成物联网系统的过程中，你作为开发者在每一步需要处理的细微差别确实很显著——我们不能期望即插即用，甚至不能期望来自你接收数据的系统具有一致的行为。

更糟糕的是，即使你怀着使其通用化以便在不同硬件设备上以相同方式运行的最好意图编写代码，你的代码也可能并非在每个平台上都能正常工作。

当然，不可能覆盖所有专用平台，也很难编写在设备级别一致的、半低层代码，而无需为每台设备进行优化。因此，我将基于这样一种理解来工作：虽然每台设备可能略有不同，并且在我们创建（和测试）解决方案时需要考虑这些差异，但代码示例在很大程度上是可移植的（有一些小的例外），并且可在大多数能够运行 Java 虚拟机或 Python 3 解释器的系统上使用。

## Living on the Edge

回顾我们有两个架构层：边缘层和云层。就结果分析而言，王国的钥匙在于云层。*可扩展性*赋予物联网真正的力量，是系统能够处理我们想要的任意多（或任意少）的能力。例如，支持物联网的可扩展云系统是我可以让单个网关设备向其发送数据，或数千个（或数百万个、数十亿个）而不会失败。

你可能已经读到过大量报告，描述物联网是数以万亿计的连接设备，这将在 {pick your favorite year post-2020} 带来某种高昂的商业价值。在云中，我（大部分）不在乎我的

服务运行在何处，只要它们始终可用并且能够处理我抛给它们的任何工作负载，无论我的边缘层中有多少设备。

云层赋予我们所有可扩展性，物联网解决方案通常在边缘表现出最大的复杂性，而异构性大多，甚至全部存在于那里。本书将重点关注的两种设备类别是*受限设备*和*网关设备*。我故意省略了“智能设备”的命名法，因为“智能”与“不那么智能”设备如何最好定义变得越来越不清晰。粗略地过度简化，受限设备在功耗、通信和处理方面存在一些限制，而网关设备则没有。

受限设备的示例是低功耗（有时由电池供电）的单板计算机（SBC），它要么从环境中读取数据（如温度、压力、湿度等），要么在被指示时触发阀门打开或关闭。网关设备的示例也是 SBC，但功能更强大且可能需要直接连接电源，具有与许多不同受限设备通信的能力，并且有足够的处理能力来汇总这些其他设备的数据，执行一些简单分析，然后确定何时应将任何相关数据发送到云端进行进一步存储和处理。

图 P-1 设想了一个概念性物联网系统架构，表示边缘层内这些设备类型之间的关系，以及云层内存在的服务和其他功能。

## Cloud Tier

Internet

End User

Cloud Services / Analytics

Data Store

## Edge Tier

Gateway Device

Sensor

Constrained Device

Actuator

*Figure P-1. - Notional IoT System Architecture*

我发现最容易将这些设备归类如下：

- *受限设备*仅处理传感、执行或两者兼有，同时仅处理针对自身的信息，必要时沿路传递信息（如果实现了正确的通信协议），并将信息发送给网关设备。简而言之，它们的处理能力有限，并且不直接连接到互联网——只能通过网关设备连接。
- *网关设备*是超集——它们可能执行与受限设备相同的工作（即它们可能连接有传感器或执行器），但也具有在边缘执行分析的能力，确定信息应如何路由（如果需要），当然，还能直接连接到互联网和使物联网对业务利益相关者有用的各种云服务。

受限设备能否直接连接到互联网？当然可以，如果它包含 TCP/IP 协议栈，具有可路由的 IP 地址且可从公共互联网访问，当然还有适当的通信硬件来与互联网通信并连接。对于本书的目的，我将受限设备的类别缩小为以下两个限制：

- 它们不直接支持到或来自公共互联网的数据包路由（尽管我们假设它们同时支持 TCP/IP 和 UDP/IP），必须与网关设备交互才能成为任何物联网生态系统的一部分。
- 它们不包含足够的计算资源来根据收集的数据智能地确定复杂的操作方案。

这意味着：随着我们制造出更小、更快、更便宜的更好计算设备；用它们与物理世界交互，并将它们（或其数据）连接到互联网以使用云服务进行处理，我们可以获得有助于实现更好业务成果的洞察。

在本书记的结尾，我将更详细地探讨像刚才讨论的那样的一些用例。每个用例都将包括一个实现练习，让你能够利用从前几章获得的知识以及你在练习中开发的代码来探索该概念。

在深入我们第一个部分之前，最后一点：每章都以一首俳句开头，试图捕捉你将学到的精髓以及你可能会遇到的一些挑战。这是怎么来的？从我早期的软件开发日子里，我工作过的一个团队有一项政策：如果你的代码提交导致夜间构建失败，你就必须写一首与问题相关的俳句并通过电子邮件发送给团队所有人。你可能在通过本书练习时不会遇到太多“构建失败”的情况，但俳句至少能在你深入每章之前提供一些轻松。

感谢阅读！

## Conventions Used in This Book

本书中使用的排版约定如下：

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
此元素表示警告或注意。

## 使用代码示例

补充材料（代码示例、练习等）可在 https://github.com/oreillymedia/title_title 上下载。

如果您对代码示例有技术问题或使用问题，请发送电子邮件至 bookquestions@oreilly.com。

本书旨在帮助您完成工作。一般而言，如果本书提供了示例代码，您可以在程序和文档中使用它。 除非您复制代码的很大一部分，否则无需联系我们征求许可。 例如，使用本书中几个代码片段编写程序不需要许可。 出售或分发 O’Reilly 书籍中的示例需要许可。 通过引用本书并引用示例代码来回答问题不需要许可。 将大量示例代码纳入产品文档中需要许可。

我们感谢，但通常不需要署名。 署名通常包括标题、作者、出版商和

ISBN。 例如：“*Programming the Internet of Things* by Andrew King (O'Reilly). Copyright 2021 Andrew King, 978-1-492-08141-8.”

如果您认为代码示例的使用超出了合理使用范围或上述许可范围，请随时联系我们 permissions@oreilly.com。

## O’Reilly Online Learning

> **NOTE**
> 
> 40 多年来，*O'Reilly Media* 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业知识。 O’Reilly 的在线学习平台为您提供按需访问实时培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频集合。 有关更多信息，请访问 http://oreilly.com。

## 如何联系我们

请将有关本书的评论和问题发送给出版商：

O'Reilly Media, Inc.

1005 Gravenstein Highway North

Sebastopol, CA 95472

800-998-9938（美国或加拿大境内）

707-829-0515（国际或本地）

707-829-0104（传真）

发送电子邮件至 bookquestions@oreilly.com 对本书发表评论或提出技术问题。

有关我们书籍和课程的新闻和信息，请访问 http://oreilly.com。

在 Facebook 上找到我们：http://facebook.com/oreilly

在 Twitter 上关注我们：http://twitter.com/oreillymedia

在 YouTube 上观看我们：http://www.youtube.com/oreillymedia

- 1 最新 UML 规范可在对象管理组织（OMG）的网站上找到（https://www.omg.org/spec/UML/）。
- 2 有关 Gordon Moore 和摩尔定律的进一步阅读，请参阅 https://www.britannica.com/biography/Gordon-Moore
- 3 https://en.wikipedia.org/wiki/ARPANET
- 4 S. D. Antón, D. Fraunholz, C. Lipps, F. Pohl, M. Zimmermann and H. D. Schotten, “Two decades of SCADA exploitation: A brief history,”
2017 IEEE Conference on Application, Information and Network Security (AINS), Miri, 2017, pp. 98-104, doi: 10.1109/AINS.2017.8270432.
- 5 有关 IETF 的 RFC 768 的详细信息，请参阅 https://tools.ietf.org/html/rfc768
- 6 有关 Tim Berners-Lee WWW 提案的进一步阅读，请参阅 https://www.w3.org/History/1989/proposal.html
- 7 在 https://www.livinginternet.com/i/ia_myths_toast.htm 查看解释
- 8 在 https://en.wikipedia.org/wiki/Trojan_Room_coffee_pot 查看解释
- 9 您可以在 https://www.rfidjournal.com/that-internet-of-things-thing 阅读更多关于 Kevin Ashton 的“物联网”演讲的内容
- 10 您可以在 https://www.computerhistory.org/timeline/2005/ 阅读 Arduino 诞生的简要总结
- 11 有关工业互联网联盟的更多信息可在其网站上找到（https://www.iiconsortium.org/index.htm）。
