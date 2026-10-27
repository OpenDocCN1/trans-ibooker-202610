

作为先决条件，我们应该已经为每个环境部署了 3 个 GKE/Anthos 集群。

Figure 14.37 Target GKE/Anthos Clusters

![](img/fb3bebce7b5f593bca1a7f8a1371be06_532_0.png)

1. 第一步是定义交付流水线，包括其推进顺序（提升序列），以及可选的运行时目标。在本示例中，我们将创建单独的目标文件。对于本示例，我们可以定义以下流水线：

### 14.4.3 delivery-pipeline.yaml

```
apiVersion: deploy.cloud.google.com/v1beta1
kind: DeliveryPipeline
metadata:
    name: web-app
description: web-app delivery pipeline
serialPipeline:
  stages:
  - targetId: test
  - targetId: staging
  - targetId: prod
```

2. 目标可以定义为单独的文件。您可以为每个目标定义一个单独的文件。最终您将拥有 3 个文件：
   - target_test.yaml
   - target_staging.yaml
   - target_prod.yaml

   在每个文件中，您定义目标的名称和目标集群。下面可以看到测试目标的示例。

现在 Cloud Deploy 已了解您的应用程序，并且它将根据您定义的提升序列管理部署到目标。

5. 现在您可以通过命令行或 CI 工具创建 **release** 来启动交付流水线。Google Cloud Deploy 会创建 **rollout** 资源，该资源将发布与第一个目标环境关联。基于该 rollout，您的应用程序将被部署到第一个目标。

从包含 Skaffold 配置的目录运行以下命令：

```
gcloud deploy releases create RELEASE_NAME --delivery-pipeline=PIPELINE_NAME
```

6. 如果您想使用 Cloud Build 作为 CI 工具，以下 YAML 文件显示了示例 Cloud Build 构建配置。这包括对 Google Cloud Deploy 的调用以创建发布，发布名称基于日期，并使用 Skaffold 进行构建。

```
- name: gcr.io/k8s-skaffold/skaffold
  args:
    - skaffold
    - build
    - '--interactive=false'
    - '--file-output=/workspace/artifacts.json'
- name: gcr.io/google.com/cloudsdktool/cloud-sdk
  entrypoint: gcloud
  args:
    [
      "deploy", "releases", "create", "rel-${SHORT_SHA}",
      "--delivery-pipeline", "PIPELINE_NAME",
      "--region", "us-central1",
      "--annotations", "commitId=${REVISION_ID}",
      "--build-artifacts", "/workspace/artifacts.json"
    ]
```

7. 一旦您准备好将应用程序部署到序列中的下一个目标，您就可以提升它。对 Google Cloud Deploy 发出调用并创建新的 rollout。

```
gcloud deploy releases promote --release=RELEASE_NAME --delivery-pipeline=PIPELINE_NAME
```

8. 您可以继续提升到最后一个环境。在示例序列中，它是生产环境。

Figure 14.38 promotion sequence

![](img/fb3bebce7b5f593bca1a7f8a1371be06_535_0.png)

9. 如果您想在提升过程中引入审批，您可以在目标定义中进行操作。作为示例，我们可能希望根据图 14.39 批准所有提升到 staging 和 production。

Figure 14.39 manual approvals

![](img/fb3bebce7b5f593bca1a7f8a1371be06_535_1.png)

您可以在每个目标定义中定义审批。

```
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name:
  annotations:
  labels:
description:
requireApproval: true
gke:
  cluster: projects/[project_name]/locations/[location]/clusters/[cluster_name]
```

参数 **requireApproval: true** 表示提升到此目标是否需要手动审批。可以为 true 或 false。此属性是可选的。默认值为 false。

10. 现在您可以批准或拒绝 rollout。要批准 rollout，请运行命令：

```
gcloud deploy rollouts approve rollout-name --delivery-pipeline=PIPELINE_NAME
```

或通过运行以下命令拒绝审批：

```
gcloud deploy rollouts reject rollout-name --delivery-pipeline=PIPELINE_NAME
```

正如您所见，这使您能够完全控制应用程序的 rollout。

### Summary of Cloud Deploy

在本节中，我们学习了如何使用 Cloud Deploy 实现应用程序的持续交付。它具有许多重要功能，如审批、日志记录和与第三方工具的集成，使其成为企业级解决方案。它与 Anthos 的端到端 CI/CD 流水线无缝集成。要了解有关 Cloud Deploy 的更多信息，请查看：https://cloud.google.com/deploy

如果您想获得 Cloud Build 的实践经验，我们鼓励您查看涵盖更多功能和集成的示例教程：https://github.com/GoogleCloudPlatform/cloud-deploy-tutorials/blob/main/tutorials/base/clouddeploy-config/skaffold.yaml.template。

## 14.5 Modern CI/CD Platform

现代 CI/CD 平台必须允许可持续的软件开发和整个应用程序交付流水线的运营。实现这一目标没有唯一的方法，并且始终取决于应用程序和组织的具体情况。

![](img/fb3bebce7b5f593bca1a7f8a1371be06_538_0.png)

Anthos 引入了集中化管理的能力，并统一了可同时使用的工具集，同时保持将外部工具集成到流水线中的灵活性。现代 CI/CD 平台的目标是支持企业级和安全的 GitOps 实现。这意味着每个组件都必须作为存储在版本控制系统中的一组配置文件和定义来描述。每组由单独的团队管理和维护。已经提到，现代 CI/CD 平台依赖于共享责任。在这种模型中，所有各方始终通过代码仓库、镜像构建和 Kubernetes 清单定义阶段等通用接口进行交互。

现代应用程序交付依赖于 Kubernetes 编排和基于微服务的架构。在这种情况下，必须在开发人员和运维人员的工作环境之间进行分离。实现取决于定义的<|message|>要求。我们可以使用单个集群中的多个命名空间用于单个站点或非生产环境。如果我们交付的服务必须具有高可用性、在全球范围内提供低延迟，或我们必须限制基础设施生命周期活动的影响，请考虑多个 Anthos 集群。多集群设置还允许我们在应用程序完全发布之前，分步骤向单个集群推出应用程序。

让我们定义一个示例应用程序作为一个简单的基于微服务的投票系统，具有可视化投票结果的能力，如下所示。

Figure 14.42 Application schema

![](img/fb3bebce7b5f593bca1a7f8a1371be06_539_0.png)

对于底层基础设施，它使用 GCP 上的 Anthos GKE 集群。在那里创建了三个命名空间，每个服务一个：

- voting
- transfer
- results

它还使用三个 Google 管理的服务：

- Cloud Memorystore 作为托管 Redis
- Cloud SQL 作为所有投票的中央数据库，以托管 PostgreSQL 的形式提供
- Secret Manager 作为安全密钥管理系统，用于存储 Memorystore 和 Cloud SQL 的凭据

开发人员使用基础设施，而不关心其交付方式。它必须满足他们与命名空间和托管服务相关的要求。负责的团队必须创建一个可用于应用程序托管目的的 CI/CD 流水线，并具有完全的灵活性来测试任何更改。

为了实现目标，我们可以使用多种不同的工具。如果我们的团队熟悉 Terraform，我们可以利用已有的知识并将其集成到我们的 CI/CD 流水线中。这种方法完全符合 DORA 的 DevOps 研究计划 [30]。它定义了在组织中实施 DevOps 的关键成功因素之一是开发人员和运维人员使用的工具选择的自由。

还有一个问题：我能否更简单地交付此类基础设施？由于我们使用的是 GCP 托管资源而不是外部工具，我们可以利用在“Anthos 配置管理”一章中详细介绍的 Anthos Config Connector 功能，将 Secret Manager、Cloud Memorystore 和 Cloud SQL 作为代码仓库中声明的对象来交付。

在需要为其他来源如 Anthos on-prem 交付服务的情况下，重要的是从第一天开始以自动化方式引入基础设施变更。每个 Anthos on VMware 或 Anthos on bare metal 都必须依赖可不断重复使用的预定义实践和安全策略。

深入到我们的现代平台基础设施，它必须交付以下元素以实现成功的应用程序交付。

