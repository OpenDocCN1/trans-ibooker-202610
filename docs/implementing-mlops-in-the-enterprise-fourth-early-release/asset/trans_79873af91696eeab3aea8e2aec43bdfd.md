
构建原型可以节省时间、降低风险并改善结果。节省时间和精力的一种极佳方式是拥有包含应用骨架和最佳实践的项目模板。从原型迁移到生产级机器学习应用可以通过多次迭代完成，每次迭代都增加更多数据、逻辑和健壮性。从第一天起将项目拆分为功能模块（例如，数据准备、[训练](https://www.iguazio.com/glossary/model-training/)、测试、服务等）并定义模块之间的接口至关重要。这允许对每个模块进行独立开发，并促进团队成员之间的更好协作。

必须定义初始原型的范围、里程碑和目标。一旦实现并可以评估目标，执行团队需要批准项目的产品化并分配其成功所需的额外资源。

## 扩展与产品化项目

为生产和扩展而设计的机器学习项目包含以下三个管道：

* `Data pipeline`：利用全量历史、运营和[实时数据源](https://www.iguazio.com/glossary/real-time-ml/)，并将原始数据转换为用于训练和推理阶段的特征。[特征存储](https://www.iguazio.com/glossary/feature-store/)可以加速数据管道的开发，并实现从其他项目复用现有数据特征。
* `Model development (CI/CD) pipeline`：自动化获取相关数据、数据验证、使用不同参数和框架进行训练、评估和测试模型、部署推理管道等流程。
* `Application pipeline`：拦截请求或事件、丰富和处理数据、使用模型进行推理、应用相关操作并监控各种资源、数据、模型和 KPI 指标。

管道必须为持续开发和运营而设计。新版本可以在不中断整体应用的情况下部署。建议以冲刺方式工作（每周、每月）。每个冲刺结束时，查看完整应用的运行情况。每个冲刺提供更多功能或健壮性，直到达到可部署且生产质量的应用。部署应用后，继续通过功能改进或错误修复进行迭代。

[图 3-3](#ch03.xhtml#mlops-engineering-flow) 说明了项目工程流程。

![](assets/ml-engineering.jpg)

在当今普遍的情况下，每个管道使用不同的框架，并由具有不同技能的不同团队维护。例如，数据管道使用 [Spark](https://spark.apache.org/)、[Flink](https://flink.apache.org/) 和 [Airflow](https://airflow.apache.org/)，模型开发使用 [Kubeflow](https://www.kubeflow.org/)，应用管道使用普通容器或无服务器函数。如此众多的工具和框架会带来运营挑战。此外，每个框架使用不同的元数据布局和 API，迫使增加额外的胶水逻辑和转换。因此，在框架间使用标准元数据和抽象对于简化并优化部署至关重要。

机器学习项目由不同的团队成员持续协作开发。因此，像 [Git](https://git-scm.com/) 这样的版本化源代码控制系统、敏捷开发流程和 CI/CD 自动化是成功结果的强制性要求。

## 项目结构和生命周期

ML 项目是针对特定机器学习应用所有工作的容器。项目托管函数、工作流、制品、笔记本、特征和配置。项目拥有所有者和成员，并具有基于角色的访问控制，应定义谁可以访问什么以及如何访问。

![](assets/ml-project-elements.png)

项目组件（参见：[图 3-4](#ch03.xhtml#ml-project-elements)）：

* `Functions`：代码元素及其包需求、配置、元数据、资源定义等。
* `Workflows`：管道（`DAG`）定义，例如哪个步骤在哪个步骤之后以及参数如何在步骤之间传递。
* `Artifacts`：项目中使用的各种数据制品（文件、数据集、模型等）的元数据和指针。
* `Notebooks`：用于交互式开发、数据探索和可视化的 [Jupyter](https://jupyter.org/) 笔记本。建议仅在笔记本中存储生产代码，因为在笔记本中测试、自动化和跟踪更改更困难。
* `Features`：特征存储特征的定义以及生成或检索这些特征的数据管道（通常称为特征集和特征向量）。
* `Configurations`：参数、密钥、构建和安装说明等。

项目应存储在源代码控制系统（`Git`）中并进行版本管理，或归档。然后，它们可以在不同的 IDE 中作为项目打开和编辑（在 `Jupyter`、 [PyCharm](https://www.jetbrains.com/pycharm/)、 [VSCode](https://code.visualstudio.com/) 等中）。这种方法支持版本控制、协作和 CI/CD。

你可以为团队定义最佳实践和项目模板。这可以简化新项目的创建，并帮助在现有项目中聚焦和优先排序。[示例 3-1](#ch03.xhtml#project-dir-struct) 演示了如何定义项目目录结构：

### 示例 3-1. 项目目录布局示例

```
my-project           # Parent directory of the project (context)
├── data             # Project data for local tests or outputs (not tracked by version control)
├── docs             # Project documentation
├── src / pkg-name   # Project source code (functions, libs, workflows)
├── tests            # Unit tests (pytest) for the different functions
├── notebooks        # directory for storing notebooks
├── README.md        # Project README
├── requirements.txt # Default Python requirements file (may have per function requirements files)
├── setup.py         # Python package setup file
├── LICENSE          # License file
└── ...
```

类似 [MLflow](https://mlflow.org/) 和 [MLRun](https://www.mlrun.org/) 的 MLOps 框架在项目目录中存储额外的元数据和配置文件（`MLproject`、`project.yaml`）。这允许从 `Git` 加载项目，自动重建其所有对象和配置，并对配置和元数据进行版本控制（又名 `Git-ops`）。

在持续开发和集成流程中（[图 3-5](#ch03.xhtml#ml-project-lifecycle) 所示），开发人员创建 ML 项目并编写和测试其代码和模型。一旦准备就绪，他们将更改推送到源代码控制系统（`Git`）。接下来，项目加载到开发集群中，在更大数据集和类似生产设置的环境上运行额外的自动化测试。然后，对代码应用错误修复，直到项目稳定并准备发布。最后，当发布准备就绪时，为项目分配版本标签，并在滚动升级过程中将该版本部署到生产集群。

![](assets/ml-project-lifecycle.png)

将项目划分为功能构建块（函数、工作流、特征等）并使用 `Git` 可实现持续协作开发。此外，将代码、ML 对象、元数据和配置放在同一个版本化项目仓库中，可简化测试、部署到生产环境以及出现问题时回滚到旧版本。

## 从 A 到 Z 的 ML 项目示例

本节演示了一个完整的 ML 项目以及从初始探索到大规模持续部署的开发流程。

开发和产品化流程包括以下步骤（[图 3-6](#ch03.xhtml#project-dev-flow) 所示）：

* 初始数据收集（用于探索）。
