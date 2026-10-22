

# 构建安全成熟度模型 (BSIMM)

![](img/cc84747f93d290964598894b4f5c153b_147_0.png)

图 5.8 BSIMM 结构和组织

截至本文撰写时，BSIMM 的当前版本为第 12 版，其中包括 9,000 名软件安全成员，既包括安全团队，也包括组织内的安全倡导者。纳入 BSIMM 12 的行业有：

- Financial
- FinTech
- Independent software vendors
- Tech
- Retail
- Insurance
- Healthcare
- Cloud
- IoT

BSIMM 包含三个与给定活动观察频率相关的成熟度级别，其中级别 1 为很少观察到，级别 3 为最常观察到。

> DEFINITION Activity 是软件安全组 (SSG) 作为实践的一部分执行或促进的行动。活动在 BSIMM 中根据观察率分为三个级别。经常观察到的活动被指定为级别 1，而较少观察和很少观察到的活动分别被指定为级别 2 和 3。

对于这些活动中的每一个，组织都可以将其态势与其特定行业中的其他组织进行衡量。虽然这不是 OWASP 对 SAMM 采取的规范性方法，但它有助于衡量组织了解其与同行相比的表现。BSIMM 的一个考虑因素是，所概述的最佳实践可能并不完全适合您的组织，但如果您发现您所在行业的大多数组织都在完成某些活动，就很难反驳。其中一个例子是识别您的个人身份信息 (PII) 义务，这意味着了解与在系统中捕获和保留 PII 相关的要求，以及在整个组织中建立 PII 清单。当参与 BSIMM 的绝大多数组织都在执行此活动时，这是一个安全的赌注，您的组织也应该这样做。

到目前为止，BSIMM 提供了五项大多数组织无论其行业如何都应追求的顶级活动。这些活动几乎已集成到 BSIMM 中的所有组织中。再次强调，同行压力是一个强大的动力，这些活动已连续多年稳居前五名。

## 表 5.2 与威胁评估实践相关的 SAMM 示例问题

| Activity Name | Domain and Practice | Description |
|---|---|---|
| Implement life cycle instrumentation and use to define governance | Governance: Strategy and metrics | This can be best defined as applying application security tools into the SDLC process in order to gather metrics as it relates to the security policies in the organization. |
| Ensure host and network security basics are in place | Deployment: Software environment | Before running your developed software, ensuring that the environment it will be deployed to is secure. That means having the systems and network that it’s deployed to properly secured. |
| Identify PII obligations | Governance: Compliance and policy | No surprise, but this is related to the data that an organization accumulates. More importantly, it’s about knowing what type of data is being stored and where it is being stored. |
| Perform security feature review | SSDL touchpoints: Architecture analysis | When architecture changes that impacts the security level or model, an architecture review should occur to understand the risk. |
| Use external penetration testers to find problems | Deployment: Penetration testing | Your internal testers and tools are great at finding certain issues, but nothing brings a security issue to the forefront quicker than an issue uncovered by an external tester. |

虽然这些活动是 BSIMM 组织中最常见的，但重要的是要看到组织在过去几个版本中增长最明显的地方的趋势。例如，过去几个版本中增长最大的前三项活动是：

- Use orchestration for containers and virtualized environments
- Ensure cloud security basics
- Use application containers to support security goals

其中每一项都从过去的版本中实现了数百个百分点的增长，这意味着越来越多的组织在过去几年开始采用这些活动。鉴于云和容器技术的采用率不断提高，这不足为奇。随着这些技术的扩展，增加其周围的安全性需求也在增长。

这些指标显示了组织的软件安全计划说明了什么？它不仅显示了组织与其同行相比的表现，还显示了行业总体上如何发展以及组织如何应对新的问题。掌握这些知识后，就该制定计划，围绕如何解决组织中已知的任何缺陷制定成熟度行动计划 (MAP)。这有助于组织通过制定提高成熟度的行动计划来解决开放的挑战和目标。目前 Synopsys 提供服务，帮助组织确定需要采取哪些步骤来构建 MAP。这首先包括通过对安全计划的评估来查看组织的当前态势。从那里定义未来状态并突出显示差距以构建路线图。最后一步是制定实现期望状态的行动计划。

正如我提到的，BSIMM 是衡量您各自行业同行的重要指标。了解 BSIMM 的一个关键方面是，如果大多数同行都在使用相同或相似的活动来解决软件安全问题，那么这是一个很好的指标，说明您也应该这样做。这就是 BSIMM 的力量。

### 5.4.3 解决您的安全不成熟问题

BSIMM 和 SAMM 采用了两种不同的方法来构建安全软件。那么哪种方法更有意义？当然，这取决于。如果您绝对需要了解自己与组织内外其他组织的衡量标准，那么进行 BSIMM 参与是有意义的。但是，如果您希望进行基本的自我评估，显示您当前的软件安全态势，那么 SAMM 是更好的选择，因为它是开放的，并且评估由组织进行。

遵循成熟度模型的最重要因素是了解您的组织能够承受什么。获得 BSIMM 评估和显示您在未来 12-24 个月内应达到的水平的 MAP 如果您的组织没有准备好进行提高成熟度的工作，将毫无价值。这意味着领导层必须接受该战略，工程组织必须与应用安全职能保持一致。更重要的是，必须将合适的资源专门用于该计划。例如，在 SAMM 和 BSIMM 中，架构分析都是一个活动。该活动需要在工程团队和应用安全团队中拥有知识渊博的资源，以便能够审查架构的安全控制有效性并减轻威胁。并非所有组织的资源都能够有效地执行这种水平的活动。

关于 BSIMM 和 SAMM 的最后比较点是要知道您是否需要或需要外部评估。由于 BSIMM 是 Synopsys 的专有产品，因此 BSIMM 评估默认是外部的。SAMM 允许您进行自我评估，但是，正如我之前所说，您需要在组织内拥有专业知识来开展该评估。如果愿意，您也可以让第三方执行 SAMM 评估。

成熟度模型没有正确答案，但这是我接下来要谈论的重要因素。有了成熟度模型，尤其是 BSIMM，更容易向组织中的利益相关者推销整体软件安全计划，并为建立民主化和分散的安全计划铺平道路，该计划可尽早将安全构建到软件中。

### 5.5 分布式应用安全

正如我到目前为止多次提到的，保护软件需要一群具有安全意识的个人和团队。应用安全资源不可能参与每个编码决策、每次设计评审和每次评估。集中式团队应用软件安全工具并创建需要在特定时间内解决的安全漏洞输出的当前方法会造成摩擦、挫败感以及对软件开发的拖累。此外，这种集中的应用安全职能成为一群试图通过告诉开发团队他们编写安全代码很糟糕来提供安全保障的团队。

Centralized vs Decentralized AppSec

![](img/cc84747f93d290964598894b4f5c153b_150_0.png)

图 5.9 应用安全集中式方法与分布式方法

采用分布式方法意味着应用安全团队将工作重点放在构建安全软件开发结构上，而不是管理工具和推动漏洞关闭的日常运营方面。这些在所有组织中仍然重要且必要，然而，应用安全团队无法扩展到所需的水平以保持始终存在。

