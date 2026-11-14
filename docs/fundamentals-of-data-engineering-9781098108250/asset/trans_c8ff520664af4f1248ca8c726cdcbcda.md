

^([5](#ch04.html#ch01fn37-marker)) Raghav Bhargava, “Evolution of Dropbox's Edge Network,” Dropbox.Tech, June 19, 2017, [*https://oreil.ly/RAwPf*](https://oreil.ly/RAwPf).

^([6](#ch04.html#ch01fn38-marker)) Akhil Gupta, “Scaling to Exabytes and Beyond,” Dropbox.Tech, March 14, 2016, [*https://oreil.ly/5XPKv*](https://oreil.ly/5XPKv).

^([7](#ch04.html#ch01fn39-marker)) “Dropbox Migrates 34 PB of Data to an Amazon S3 Data Lake for Analytics,” AWS website, 2020, [*https://oreil.ly/wpVoM*](https://oreil.ly/wpVoM).

^([8](#ch04.html#ch01fn40-marker)) Todd Hoff, “The Eternal Cost Savings of Netflix's Internal Spot Market,” High Scalability, December 4, 2017, [*https://oreil.ly/LLoFt*](https://oreil.ly/LLoFt).

^([9](#ch04.html#ch01fn41-marker)) Todd Spangler, “Netflix Bandwidth Consumption Eclipsed by Web Media Streaming Applications,” *Variety*, September 10, 2019, [*https://oreil.ly/tTm3k*](https://oreil.ly/tTm3k).

^([10](#ch04.html#ch01fn42-marker)) Amir Efrati and Kevin McLaughlin, “Apple's Spending on Google Cloud Storage on Track to Soar 50% This Year,” *The Information*, June 29, 2021, [*https://oreil.ly/OlFyR*](https://oreil.ly/OlFyR).

^([11](#ch04.html#ch01fn43-marker)) Evan Sangaline, “Running FFmpeg on AWS Lambda for 1.9% the Cost of AWS Elastic Transcoder,” Intoli blog, May 2, 2018, [*https://oreil.ly/myzOv*](https://oreil.ly/myzOv).

^([12](#ch04.html#ch01fn44-marker)) 示例包括 [OpenFaaS](http://www.openfaas.com)、[Knative](https://oreil.ly/0pT3m) 和 [Google Cloud Run](https://oreil.ly/imWhI)。

^([13](#ch04.html#ch01fn45-marker)) Justin Olsson and Reynold Xin, “Eliminating the Anti-competitive DeWitt Clause for Database Benchmarking,” Databricks, November 8, 2021, [*https://oreil.ly/3iFOE*](https://oreil.ly/3iFOE).

^([14](#ch04.html#ch01fn46-marker)) 关于此类经典案例，请参阅 William McKnight and Jake Dolezal, “Data Warehouse in the Cloud Benchmark,” GigaOm, February 7, 2019, [*https://oreil.ly/QjCmA*](https://oreil.ly/QjCmA)。

# 第二部分 深入数据工程生命周期

# 第 5 章 源系统中的数据生成

欢迎来到数据工程生命周期的第一阶段：源系统中的数据生成。正如我们之前所述，数据工程师的工作是从源系统获取数据，对其进行处理，并使其能够服务于下游用例。但在获取原始数据之前，你必须了解数据存在于何处、是如何生成的，以及其特性和怪癖。

本章涵盖了一些常见的运营源系统模式和主要的源系统类型。用于数据生成的源系统有很多，我们不会穷尽覆盖所有系统。我们将探讨这些系统生成的数据，以及在使用源系统时应考虑的事项。我们还将讨论数据工程的潜流如何应用于数据工程生命周期的这一第一阶段（[图 5-1](#ch05.html#source_systems_generate_the_data_for_th)）。

![](assets/fode_0501.png)

随着数据的激增，特别是随着数据共享的兴起（将在下文讨论），我们预计数据工程师的角色将发生重大转变，更多地转向理解数据源与数据目的地之间的相互作用。数据工程的基本管道任务——将数据从 A 点移动到 B 点——将大幅简化。另一方面，理解数据在源系统中创建时的本质仍然至关重要。

## 数据来源：数据是如何创建的？

当你了解生成数据的各种系统的底层运营模式时，理解数据是如何被创建的至关重要。数据是一组无序的、无上下文的事实和数字。它可以通过多种方式创建，无论是模拟还是数字。

*模拟数据* 创建发生在现实世界中，例如语音、手语、纸上书写或演奏乐器。这种模拟数据通常是瞬态的；你有多少次进行过口头交谈，其内容在交谈结束后就随风而逝？

*数字数据* 要么是通过将模拟数据转换为数字形式而创建，要么是数字系统的原生产物。模拟转数字的一个例子是手机短信应用将模拟语音转换为数字文本。数字数据创建的一个例子是电商平台上的信用卡交易。客户下单，交易记入其信用卡，交易信息被保存到各个数据库中。

在本章中，我们将利用一些常见示例，例如与网站或移动应用交互时创建的数据。但事实上，数据无处不在。我们从物联网设备、信用卡终端、望远镜传感器、股票交易等捕获数据。

熟悉你的源系统及其生成数据的方式。花精力阅读源系统文档，了解其模式和怪癖。如果你的源系统是 RDBMS，了解其运作方式（写入、提交、查询等）；深入了解可能影响你从中摄取数据的源系统细节。

## 源系统：核心概念

源系统以多种方式生成数据。本节讨论你在使用源系统时将频繁遇到的核心概念。

### 文件和非结构化数据

*文件* 是字节序列，通常存储在磁盘上。应用程序经常将数据写入文件。文件可能存储本地参数、事件、日志、图像和音频。

此外，文件是数据交换的通用媒介。尽管数据工程师希望能以编程方式获取数据，但世界上仍有大量数据通过文件发送和接收。例如，如果你从政府机构获取数据，极有可能你会以 Excel 或 CSV 文件的形式下载数据，或通过电子邮件接收文件。

作为数据工程师，你将遇到的主要源文件格式——无论是手动创建还是作为源系统进程的输出——包括 Excel、CSV、TXT、JSON 和 XML。这些文件各有怪癖，可以是结构化的（Excel、CSV）、半结构化的（JSON、XML、CSV）或非结构化的（TXT、CSV）。虽然作为数据工程师你会大量使用特定格式（如 Parquet、ORC 和 Avro），但我们稍后会介绍这些，这里重点关注源系统文件。[第 6 章](#ch06.html#storage-id000094) 涵盖文件的技术细节。

### API

*应用程序编程接口* (API) 是系统间交换数据的标准方式。理论上，API 简化了数据工程师的数据摄取任务。实际上，许多 API 仍然暴露出大量数据复杂性供工程师管理。即使随着各种服务、框架以及用于自动化 API 数据摄取的服务的兴起，数据工程师往往仍需投入大量精力来维护自定义 API 连接。我们将在本章后面更详细地讨论 API。

### 应用数据库（OLTP 系统）

*应用数据库* 存储应用程序的状态。一个标准示例是存储银行账户余额的数据库。随着客户交易和付款的发生，应用程序更新银行账户余额。

通常，应用数据库是*联机事务处理* (OLTP) 系统——一种以高速率读写单个数据记录的数据库。OLTP 系统常被称为*事务型数据库*，但这并不一定意味着相关系统支持*原子事务*。

更广泛地说，OLTP 数据库支持低延迟和高并发。RDBMS 数据库可以在不到一毫秒的时间内选择或更新一行（不计网络延迟），并每秒处理数千次读写。文档数据库集群可以管理更高的文档提交率，但以潜在的不一致性为代价。一些图数据库也可以处理事务型用例。

