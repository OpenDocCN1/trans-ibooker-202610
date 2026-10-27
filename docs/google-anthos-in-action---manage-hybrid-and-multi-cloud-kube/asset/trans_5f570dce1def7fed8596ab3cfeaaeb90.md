

- ACM还基于开源工具引入了其他组件：
  - Config Connector为Kubernetes集群提供基础设施即数据能力，通过声明Kubernetes资源来配置Google Cloud资源。
  - Policy Controller基于开源Open Policy Agent，为管理员提供了一种便捷工具，可在其集群中创建和执行策略。
  - Hierarchy Controller是一项计划的结果，该计划旨在提供一种在集群命名空间之间复制资源的替代方法。由于将密钥存储在git仓库中是一种反模式，Hierarchy Controller定义允许组织定义一次密钥或配置，然后自动将其复制到后代命名空间。
- 三个案例研究探讨了使用和实现ACM的不同原因：
  - Evermore Industries希望减少直接在其生产环境中工作的用户数量，并允许开发团队直接配置某些Google Cloud资源。他们目前正在使用，并将继续使用模板引擎来生成其Kubernetes配置。
  - Village Linen, LLC在本地和异地都使用ACM，并使用Hierarchy Controller管理两组集群中ConfigMap和Secret的复制。
  - Ambiguous Rock Feasting精通Kubernetes，但特别使用ACM的ClusterSelectors功能，以便更精确地控制其应用程序的部署位置。

[1] Anthos UI主要在第三章“Anthos，单玻璃面板UX”中介绍。

[2] 有关Kubernetes操作符模式的更多信息，请参见 https://kubernetes.io/docs/concepts/extend-kubernetes/operator/

[3] 注解为`configmanagement.gke.io/managed: enabled`。

[4] https://cloud.google.com/anthos-config-management/docs/how-to/multi-repo

[5] 这与Deployments和Jobs使用的模式相同，详见 https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/

[6] https://cloud.google.com/config-connector/docs

[7] https://cloud.google.com/anthos-config-management/docs/concepts/policy-controller

[8] 有关RBAC的更多信息，请参见“Anthos，基于Kubernetes的计算环境”一章。

[9] https://www.openpolicyagent.org/docs/latest/kubernetes-introduction/

[10] https://cloud.google.com/anthos-config-management/docs/concepts/hierarchy-controller

[11] https://github.com/kubernetes-sigs/multi-tenancy

[12] 有关CI/CD和Anthos的更多信息，请参见“Anthos，与CI/CD的集成”一章。

[13] Istio在“Anthos Service Mesh：规模化安全与可观测性”一章中进行了详细探讨。

## 14 Anthos，与CI/CD的集成

**作者：Konrad Clapa和Jaroslaw Gajewski**

### 本章涵盖

- 理解CI/CD概念
- 自动化持续开发工作流
- 为你的Anthos应用引入持续集成
- 使用Cloud Deploy管理持续部署
- 理解现代CI/CD平台

在本章中，我们将指导你如何开发和部署Anthos应用程序。为简化操作，我们将使用一个简单的“Hello world”应用程序。我们将同时使用**Python**和**Go**语言的示例。我们将走完整个软件工作流。这将从持续开发开始，我们将学习如何开始开发Anthos应用程序，甚至在将代码提交到Git仓库之前预览它们。然后我们研究持续集成，最后讨论持续开发/交付。

我们需要认识到，可能有三种不同角色与CI/CD流水线交互。这些角色是：

- **开发人员：** 开发应用程序代码
- **运维人员：** 使用Kubernetes清单配置应用程序部署
- **安全人员：** 配置策略以确保Kubernetes部署的安全

在本章中，我们将重点放在前两个角色上。如果你想了解更多涉及策略的安全角色，请参阅*配置管理章节*。

**图14.1 CI/CD工作流**

现代软件开发过程很复杂。以可持续的方式、以一致的速度生产高质量软件，涉及许多流程和工具。实施CI/CD流水线是实现这一目标的最佳实践之一。**持续集成（CI）**是一种软件开发实践，开发人员频繁地检入代码，定期集成——至少每天一次，每次集成后都进行验证；也就是说，通过自动化过程来构建和测试集成变更[^1]。它使我们能够以恒定的速度和适当的水平实现可靠、可重复和可复用的构建，同时防止混乱并提高效率。

持续集成只是软件交付过程的一个方面，对于成功的流水线驱动开发，CI之后必须进行持续交付（CD）。**持续交付（CD）**[^2]是指以可持续的方式，安全且快速地将所有类型的变更（包括新功能、配置更改、错误修复和实验）投入生产的能力。它适用于基础设施配置、应用程序部署、移动应用发布、数据库和静态资源修改。持续交付可以用于任何环境，无论特定组织的监管要求如何。持续交付流水线通过使流程更加自动化，改善了从源代码到生产的软件交付，从而提高了流水线的可靠性、可预测性和可见性，降低了风险[^3]。

让我们看看现代CI/CD平台的一些特征：

### 可重复性

允许自动化围绕所创建的代码和制品的要求和流程。构建过程应该是确定性的，以便开发人员对生产的制品有信心。可重复的构建和测试还允许开发人员在其本地环境中运行相同的过程。部署和配置管理的自动化有助于在环境中提供一致性。

### 可靠性

提高开发和运维团队对流程和系统的信心，这些流程和系统保证工具的可用性和适用性，以及集成、测试和运维要求的完整性和充分性。通过定义的工作流进行自动化测试是捕获和跟踪组件最终成功和失败状态的关键，在开发和发布周期中提高团队信心和知识。

### 可复用性

使团队能够扩展、简化和加速开发工作流。CI/CD流水线的实现方式应允许将流水线组件复用于类似应用程序。这不仅降低了设置新流水线的成本，还提高了开发人员在整个企业中处理多个应用程序时的效率。

### 自动化测试

在高质量交付流程中，验证所开发的系统架构和功能至关重要。这可以通过将健壮的自动化测试流作为持续交付流水线的一个组成部分来实现。现代交付流水线应尽可能多地包含自动化测试。这些不仅包括单元测试、组件测试、系统功能测试，还包括检查能力、可用性和安全合规性的非功能测试。自动化测试几乎可以立即向开发人员提供反馈，从而减少生产部署中的缺陷数量和错误率。

### 基于主干的开发

当开发人员在“脑裂”环境中工作时，持续交付可能会显著减慢，在这种环境中，功能或错误修复代码分支的寿命非常长。因此，在大型团队中，代码变更在集成长期存在的分支时可能会引起冲突。这可能需要手动操作，使CI流程停滞。

### 环境一致性

环境一致性是降低风险的关键方面之一

