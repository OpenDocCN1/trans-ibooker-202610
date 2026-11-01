
| 1K 记录 |  |  |  |  |
|---|---|---|---|---|
| Redshift 数据 > 1K 记录 | `copy function` | `copy_from_files` | `unload or read_from_sql` | `unload_to_files` |

表 3.1：RedShift 的函数

除了使用前述的 `redshift-connector` 方法之外，我们还可以使用 Redshift DataAPI 连接到 Redshift 并通过 `awswrangler` 执行查询。

## CHALLENGE

- 你可以使用 Redshift Upsert 模式来更新现有记录并插入新记录，与我们在 RDS 中所做的类似。这将帮助你在数据管道中使用 `awswrangler` 按计划频率将数据加载到 Redshift。

## Summary

在本章中，我们学习了如何使用 AWS SDK for pandas（即 `awswrangler`）。我们探索了 `awswrangler` 库中可用的各种组件，以及它如何使 pandas DataFrames 更接近 AWS 生态系统。我们学习了如何为不同的用例和开发环境自定义和安装该库。我们还查看了 `awswrangler` 与 AWS 服务（如 Amazon S3、Amazon RDS、Amazon Redshift 和 Amazon Athena）的集成，以及 `awswrangler` 中可用的不同功能。

下一章中，我们将学习 SageMaker Data Wrangler，它有助于我们在 ML 管道中执行数据整理活动。

## Introduction to SageMaker Data Wrangler

数据处理是 **机器学习**（**ML**）不可或缺的一部分。事实上，说 ML 模型的性能仅取决于用于训练它们的数据并不夸张。根据 2016 年的 *Forbes* 调查，ML 工程项目中 80% 的时间都花在数据准备上。这是一个惊人地高的时间占比。为什么会这样？由于现实世界中数据的固有特性，数据准备既繁琐又资源密集。在 ML 中，这类现实世界的数据通常被称为脏数据、不干净数据、噪声数据或原始数据。在几乎所有情况下，这就是你开始 ML 流程时所使用的数据类型。即使在你认为数据良好的罕见情况下，你仍需确保其格式正确并进行缩放以使其有用。在这种原始数据上应用 ML 算法不会得到高质量的结果，因为它们无法正确识别模式、检测异常，或在开发和训练环境之外足够好地泛化。

在当今互联和数字化的世界中，数据的生成速度比其被消费和处理的速度更快。所产生的大量数据可以成为构建 ML 模型的真正优势，这些模型用于以前未被探索或被认为不可能的应用。然而，这也带来了一些挑战。让我们接下来看看其中的一些。

主要挑战之一是数据量。由于生成的数据量巨大，我们需要能够摄取、处理和存储相关属性的框架和工具。我们需要能够稳健运行并可自动扩展以满足数据摄取需求的工具。下一个挑战是数据质量。在现实世界中，数据几乎从不完美；你必须处理诸如不准确、不一致、缺失数据和多种数据格式等挑战，等等。在这里，我们需要能够准备和清理这些数据的工具，以便将其用作 ML 管道的输入。由于大量数据清理是可重复且耗时的，这些工具应原生支持可重用性，以便数据处理从业者能够以尽可能少的阻力有效地操作。数据从业者很少单独工作。他们通常是组织内更大团队的一部分，并且经常需要与其他跨职能团队协作。同样重要的是，他们用于数据处理的工具应支持这种协作并与组织现有的工具和存储库集成。

近年来，我们有意努力构建负责任的 ML 模型——公平且无偏见的模型。这是一个难以解决的问题。之所以在现实世界数据中难以解决的一个主要原因是此类数据中可能存在的固有偏见。这些偏见可以表现为某些类别的不平衡或代表性不足。偏见也可能由于特定数据源的选择而渗入。此外，某些数据集中的历史偏见可能会进入你的 ML 数据。检测偏见也很困难，因为它并不总是能通过标准的数据可视化和分析工具看到。你通常需要计算额外的指标来挖掘这些隐藏的偏见。这再次可能既繁琐又耗时，如果手动完成的话。即使是脚本化或编程实现，也可能出错。如果这些偏见未被检测到且未得到纠正，它们可以直接转移到在该数据上训练的 ML 模型上，从而导致现实世界的 ML 模型存在偏见。虽然重要的是要理解，在数据科学中确实存在需要偏见才能使我们的 ML 模型在现实世界中很好泛化的场景，但本书的范围不包括此内容。目前，我们将假设数据中的偏见是我们需要在数据准备过程中检测和纠正的东西。因此，我们用于 ML 的数据准备和处理工具必须能够原生地检测和修复原始数据中的这些偏见。ML 的数据准备涉及多个步骤，例如数据转换、特征工程以及数据可视化和验证。ML 的数据准备涵盖了将数据转换为可用于训练 ML 模型格式所需的一切。更具体地说，数据准备包括数据预处理和特征工程。虽然独立的数据准备工具的采用率不断增长，但数据准备仍然繁琐、耗时且复杂。这些工具中的大多数通常缺乏集成框架，而集成框架对于构建 ML 管道至关重要。将数据准备与 ML 管道集成的能力确保了这些管道是可重复、可重用和可部署的。这进一步使 ML 工程师能够构建 **ML 运维**（**MLOps**）管道。

正如你在前几章中看到的，**亚马逊网络服务 (AWS)** 提供了各种专为数据处理而构建的工具，例如 Glue 和 **弹性 MapReduce (EMR)**。虽然迄今为止我们所涵盖的所有工具都可用于 ML，但在本章中，我们将介绍 SageMaker Data Wrangler，这是一个专为 ML 而设计的工具。SageMaker Data Wrangler 是一种数据处理工具，使数据科学家能够专门为 ML 导入、转换和分析数据。SageMaker Data Wrangler 拥有超过 300 种内置转换，常用于处理 ML 数据。借助 SageMaker Data Wrangler，数据科学家可以创建自动数据编排工作流并生成数据质量洞察。使用 Data Wrangler，你可以轻松地将实验代码投入生产。无需手动筛选和翻译数百行数据准备和摄取代码，你可以直接导出准备好的数据。你可以从多个数据源导入数据，例如 **亚马逊简单存储服务 (Amazon S3)**、Amazon Athena、Amazon Redshift、Snowflake 和 Databricks，并使用超过 300 种内置数据转换和代码片段库处理数据，以便在无需编写任何代码的情况下快速规范化、转换和组合特征。你也可以使用 PySpark、**结构化查询语言 (SQL)** 或 pandas 引入自定义转换。事实上，你甚至无需导入数据集即可开始探索 Data Wrangler。Data Wrangler 的一项新发布功能使你能够通过使用示例数据集并遵循建议的操作来更快地开始试验 Data Wrangler，从而轻松首次导航产品。处理完原始数据后，你可以直接将其导出到 S3 或直接导出到诸如 SageMaker Feature Store 之类的特征存储。

Data Wrangler 支持额外的集成，允许你与 Snowflake 等其他数据存储集成，从而使数据科学家能够以更少的努力和时间完成数据准备、清理和特征工程任务。除了直接导出处理后的数据外，你还可以通过单击即可将所有数据准备工作流直接导出到笔记本中，以便轻松投入生产。将整个数据处理流程作为代码导出的能力在将数据处理与多个工作流自动化和编排工具（如 Apache Airflow、SageMaker Pipelines 和 AWS Step Functions 等）集成时非常有用。换句话说，SageMaker Data Wrangler 可以减少聚合和准备 ML 数据所需的时间，并直接使数据科学家能够花更多时间创建 ML 模型。总的来说，Data Wrangler 不仅为你提供强大的数据可视化和数据转换功能，还能让你非常快速地将工作流投入生产。正如名称所示，Amazon SageMaker Data Wrangler 是 Amazon SageMaker Studio 中的一个功能。为了明确起见，曾经存在 SageMaker Studio 之外的 Data Wrangler 开源版本，现已更名为 AWS SDK for pandas。Amazon SageMaker Studio 是一个基于 Web 的 **集成开发环境 (IDE)**，用于 ML。SageMaker Studio 提供了数据科学家构建、训练和部署 ML 模型所需的所有工具。SageMaker Studio 具有多项功能，可进行偏见检测、数据处理、**自动化 ML (AutoML)**、ML 训练、模型部署和 MLOps。因此，要从 SageMaker Studio 使用 SageMaker Data Wrangler，你必须首先确保已设置 Amazon SageMaker Studio。虽然 SageMaker Studio 提供的所有功能的详细概述超出了本书的范围，但我们在本章末尾包含了 *SageMaker Studio setup prerequisites* 部分，以便你快速设置并开始使用 SageMaker Data Wrangler。在本书撰写时，SageMaker Data Wrangler 可处理表格和时间序列数据。综上所述，Amazon SageMaker Data Wrangler 是 Amazon SageMaker 的一项功能，可通过可视化界面使数据科学家和工程师更快地为 ML 应用准备数据。Data Wrangler 将聚合和准备 ML 数据所需的时间从数周缩短到数分钟。SageMaker Data Wrangler 的核心功能包括纠正数据问题，例如数据不平衡和缺失数据，以及专门为 ML 转换数据。在下一节中，我们将介绍 SageMaker Data Wrangler 的一些核心功能。

## Data import
