## 第四部分

## 运行阶段

在本书记的最后一部分，我们进入生命周期的运行阶段，在此阶段通过实施流程和自动化采取行动。我们还深入探讨建立加速组织内 FinOps 的合作伙伴关系。最后，我们到达 FinOps 的终极状态：数据驱动的决策制定和 FinOps 成功的关键要素。

## 运行：使团队与业务目标保持一致

没有行动流程，目标永远无法实现。当你进入 FinOps 的运行阶段时，你将致力于支持和加强前两个阶段中确定的要素。

在本章中，我们更广泛地审视流程，以及它们如何不仅能够实现自动化，还能帮助组织实现 FinOps 目标。

## 实现目标

优化阶段设定目标，而运行阶段采取行动。这里不会设定新目标——这是基于商业案例和背景，对已设定的目标做出决策并制定计划的阶段。

让我们以在非工作时间关闭资源——当不需要时——的概念为例，并应用正确的操作顺序。在告知阶段，你识别出可能受益于关闭的资源。在优化阶段，你衡量潜在节省并根据这些指标设定希望节省的目标。在运行阶段，你定义资源开关的流程，并且可以启用自动化以提高资源应用新流程的可能性。这是引入可扩展性和复制性的阶段，通过定义可在整个组织中实施的流程来实现。

运行阶段并不总是导致行动。如果将优化阶段视为识别优化机会并为行动构建小型商业案例的阶段，那么运行阶段就是你决定是否采取行动的阶段。

> 大多数信息都是无关的。知道忽略什么可以节省你的时间，减少压力，并改善你的决策。
> 
> —Shane Parrish, Farnam Street

随着生命周期每次迭代，另一个目标会逐渐聚焦，并在此基础上构建新的流程和自动化以实现该目标。在构建流程时，你可能会发现更多想要实现的目标。当这种情况发生时，返回告知和优化阶段以正确衡量并为新流程设定目标。保持每次生命周期的迭代小而快，以便正确衡量每次更改。

## 人员配置与扩充 FinOps 团队

随着时间的推移，大多数组织会聘请经验丰富的 FinOps 从业者来组建内部团队，正如我们在第 3 章中所讨论的那样。然而，近年来该领域的职位数量激增，超过了市场上经验丰富人才的供应。IDC 在 2022 年估计，60% 的企业已经拥有或正在组建 FinOps 团队。人才竞争可能非常激烈。

许多组织最初会转向 FinOps 顾问来扩充和启动其团队，同时在内部通过 FinOps 培训提升技能，并在外部吸引人才。所有主要全球咨询公司和大型会计公司现在都拥有 FinOps 实践，包括 Accenture、Deloitte、EY、KPMG、PWC 等，以及系统集成商包括 HCL、SADA、SoftwareONE、Virtasant 等。

然而，这些实践在成熟度和规模上差异很大，因此请务必确认他们是否为 FinOps 认证服务提供商，并核实他们配备的 FinOps 认证专业人员的数量。

## 流程

在生命周期循环的第一次，多数组织专注于了解当前成本。这包括围绕拥有哪些账户建立流程，识别账单的组织方式，并实施某种形式的支出分析工具，无论是直接来自云服务提供商还是来自第三方 FinOps 平台，以实现更高级别的能力，如完整的分摊计费或容器报告。

在此之后，你的组织最重要的目标应驱动你构建的流程。成本敏感型组织会将成本效率作为最重要的目标，而专注于快速创新的组织则会选择专注于实现速度。



Many processes work together to formulate a successful FinOps practice. Automation follows process.

![](img/4d4736bc293fd892d1da81802d0f9520_351_0.png)

Before you can build automation, you need to build out processes for that automation to follow.

It’s important to define who is responsible for (or “owns”) the process, what parts of the organization must follow the process, and to what goal or goals the process contributes. A clearly defined operating model outlines expectations for staff.

When you adopt third-party automation tools—covered in Chapter 21—they often dictate how the process must work. When using these tools, you may need to adjust your defined processes to suit.

Start by figuring out how items get added to the new process (onboarding), what is expected from teams (responsibility), how teams are informed about when the process should be followed and the outcomes of following the process (visibility/alerting), and finally begin following the process (action).

## Onboarding

Circumstances change, especially when you’re using the cloud. Your organization might add to its existing cloud deployments or go through a company merger. All processes should clearly define what changes with cloud growth; otherwise, you end up with a process that works for the present but not for the near future. Don’t just think about the now, think about what the future may bring, and start thinking ahead about which parts of the process will need to evolve so you’re setting yourself up for future changes and success. Getting buy-in from the teams that will be actioning the process should happen well before action needs to be taken, and setting expectations with them that there will be changes as the practice matures. Convey to teams the goal and outcomes that the new process is aiming to achieve.

### Stories from the Cloud—Mike

At Atlassian, there are specific processes for onboarding new cloud accounts. These processes define how an account is added to the existing billing structures and how FinOps tooling is added to the new account. Governance tooling is added to the account, enabling idle resource management and rightsizing recommendations. Any existing RIs in the account are merged into the centralized management by the FinOps team. Having this governance builds a standard approach to account management, removing the need for individual teams to manage their RIs, and allows for more savings over time. The items being governed will evolve as the FinOps practice matures, such as when we moved from using Trusted Advisor’s compute optimizations to using Compute Optimizer’s recommendations. This allowed us to cover more cloud services beyond the basic compute. The central team responsible for governance is constantly working on providing better tooling to work with the next order of magnitude of scale.

## Responsibility

Each process should be allocated to an owner. Effective processes clearly define who does what, and when, and how they do it. Do teams follow the process daily, weekly, monthly, or at some other frequency? Remember some processes will be followed by the FinOps practitioner (like buying commitments), while others are distributed (like rightsizing and usage optimization).

Responsibilities assigned to teams will grow as processes mature. Regularly communicating any increases in expectations will reduce the likelihood of teams claiming they were unaware of what they should have been doing and when.

Through each iteration of the FinOps lifecycle, you build on these processes. At the beginning of the cost visibility journey, for example, the only expectation of a team when they receive a cost report might be to read it. Later, teams might be asked to investigate and explain any costs over a certain threshold. Even later, teams might be required to revise budgets and forecasts when projections show goals won’t be met.

Once again, you’re gradually maturing FinOps. It’s impossible to have teams investigate costs during the first time through the FinOps lifecycle without first putting in place a well-developed cost allocation strategy and correctly forecasted budgets. Instead, you minimize time through the loop, because trying to build all the processes at once makes it impossible to track the impacts of each process.

![](img/4d4736bc293fd892d1da81802d0f9520_352_0.png)

In photography studio and theater lighting design, there’s a maxim that reminds the lighting technician to only move one light at a time—that way, they can see the impact of each change before making additional adjustments. The same holds true here: if you change processes related to your rightsizing or tagging and to your reserved instance or savings plan management at the same time, you may not be able to easily identify which change drove movement in your bill.

## Visibility

Both before and after an action is taken, you must build visibility. In the operate phase, processes that enable visibility ensure that everyone is aware of the changes required and those being made. The most common operational processes for visibility send automatically generated reports or alerts to teams to make them aware of events as they happen.

Reports should be clear and easy to understand using the FinOps common language we defined in Chapter 4. Training and documentation should be made available to ensure all staff are familiar with this lexicon. Alerting and anomaly detection should occur on predictions based on current metrics, allowing staff to respond before things are off track. Instead of waiting for a target line to be breached, forecast the near term to identify when action is required early. A good example of this is AWS Budgets, which allows alert configuration based on predicted end-of-month spend.

Over time, processes and communication channels will grow in importance and sophistication. The FinOps team can bring development teams, finance teams, and business teams to the table to learn what information can help them plan and make decisions more effectively, as well as to receive information about their plans. Over time, the amount and variety of information provided will increase, and it will be critical for FinOps practitioners to continuously improve how they communicate.

Many FinOps teams start by holding monthly or quarterly meetings with the technology teams building and running applications. The first meetings can be awkward, uncomfortable, and somewhat guarded, with neither side quite understanding what the other wants. Over time, as these discussions continue, the FinOps team works with the IT teams to advocate for them, to evangelize the good steps they are taking to understand and control costs, and to provide them with a better understanding of business goals and finance requirements. Records of these meetings show deeper conversations about complex cloud decisions the team is considering, and participants begin to come to the meeting prepared to talk about ideas to save costs, rather than being dragged through their own cost data.

Combining visibility with clear responsibility and role definition breeds trust and leads to partnership in action. Working with the various teams of your organization is one of the processes the FinOps team needs to build. Be careful not to overlook the processes related to operating a FinOps team successfully when building processes such as rightsizing and commitment-based discount program planning, which are more focused on the cloud providers.

## Action

The action stage is where something actually happens—processes involving activities that enable your organization to reach its FinOps goals, from generating new reports to turning actual cloud resources on and off. Action processes are often good candidates for automation. When we discussed rightsizing in Chapter 15, we highlighted that recommendations are often generated by the centralized FinOps team and then distributed to engineering teams so they can investigate and adjust their resources where appropriate. Without clear processes for how rightsizing recommendations are

生成或关于对工程师期望的沟通不充分时，合理配置资源往往不会成功。

Central FinOps 团队可以成为关于如何以不断提升的精细度执行各种 FinOps 能力的信息的绝佳来源。内部知识共享、流程库和工具对大型或复杂的组织而言可以带来巨大益处。脚本、流程文档和操作指南可以被收集供所有人使用，同时附带已采取行动或做出的决策的记录和日志。请记住，任何组织内的不同团队都处于不同的成熟度水平和 FinOps 生命周期阶段。他们应该有一个明确的地方去寻找行之有效的流程。

直接行动可能并不总是你做出的决定。即使你有出色的合理配置资源的商业案例，如果关键员工需要专注于另一项战略举措，也可能不可行。你可能会将一项承诺采购推迟两周，以配合关于云架构的待定决策，而该决策可能会彻底改变所考虑服务的需求。并非每个优化机会都会导致行动或具体的节省。

然而，同样重要的是要记录这些决策及其制定理由，以提供行动记录并指导未来的决策。

## How Do Responsibilities Help Culture?

定义哪些团队负责采取行动的流程也有助于建立不同团队协作的文化。团队应了解他们的贡献如何帮助组织实现目标。当团队看不到自己的努力如何产生贡献时，他们往往不会遵循流程。

## Carrot Versus Stick Approach

有助于突出遵循流程积极影响的报告会鼓励团队付出努力以实现这些收益。跟踪并报告团队未遵循流程的地方以及衡量这些影响也很重要。建立最佳表现者和最差违规者名单可以帮助组织实现其目标。这些报告在组织内部的传播方式将由公司的文化决定。有些公司公开奖励成功并私下处理投入不足的团队，而其他公司则更喜欢公开的“最差违规者”榜单。

虽然传统的 IT 组织通常使用积极指标而非消极指标，但在公有云和 FinOps 的世界里有一个巨大的区别：团队有能力以他们在数据中心运行时从未有过的方式阻止浪费和负面行为。因团队在成本早已折旧的存储区域网络（SAN）上浪费存储容量而惩罚团队意义不大，但突出那些持续每小时花费公司金钱的未使用资源，尽管它们只需几次点击即可终止，这是有意义的。

在使用优化中，常见的问题是让团队实施变更。展示你的团队可以实现的总体节省以及其他团队已产生的节省，有助于说服团队参与流程。

这里的偏好是使用胡萝卜的方式与工程师建立协作，避免制造我们对他们的环境。不幸的是，前述的胡萝卜并不总是足够，你需要与不遵循 FinOps 指导的团队合作。

![](img/4d4736bc293fd892d1da81802d0f9520_355_0.png)

请记住，有些团队预计花费会显著高于其他团队。基于总支出的“最差违规者”名单不会有成效。虽然团队 A 花费比团队 B 多可能是个话题，但更重要的是知道团队 B 是否超预算而团队 A 是否达标。将对话集中在关键事项上，而不对不重要的事项浪费时间，这符合 FinOps 的语言。

## Handling Inaction

对于执行团队而言，传播实现目标对整个组织成功的重要性信息很重要。当团队理解这一点时，他们会更关注对其整体绩效的报告。当团队想要以速度换取支出时，管理层需要批准。此例外应纳入预算考虑。

你很少会遇到不在乎组织超支的员工。因此，当我们发现团队不遵循 FinOps 团队的建议时，最常见的原因是他们有其他优先事项。了解这些优先事项是什么可以避免意见冲突。

团队可能会更关注执行速度而非成本。更早完成项目可能会给业务带来整体市场优势。如果团队在已部署的内容上遇到问题，为了省钱而阻止他们解决这个问题可能不符合业务利益。

如果团队成员因其他项目时间表而不愿分配时间来达成目标，那么负责路线图规划的管理层需要专门分配时间用于优化，或者预算负责人需要记录因未分配时间进行优化而导致的支出增加。

在有正当理由超出目标的情况下，目标设定阶段可能会遗漏细节。花更多时间进行目标设定可以防止团队出现在“最差违规者”名单中。

当团队不优先处理其目标且不愿做出必要的变更时，你该怎么办？这时，FinOps 实践者会告知业务部门的预算负责人哪些目标未达成以及有哪些机会可以帮助团队重回正轨。

## Putting Operate into Action

让我们看一个通过移除闲置资源进行使用优化的例子。FinOps 团队定义了一个检测闲置资源并估算潜在节省的流程。在 FinOps 生命周期中的优化阶段，FinOps 实践者衡量闲置资源的量。

假设你有 $150,000 的潜在成本规避，其中设定目标为 $50,000。你定义一个流程，分配责任并明确说明对工程团队的期望、他们应多快响应建议，以及团队应如何将其资源排除在流程之外。你为这些闲置资源生成报告并发布给负责这些资源的团队。

随着团队移除闲置资源，可避免的成本金额从最初的 $150,000 下降。工程团队也会看到分配给他们的建议数量在下降。这两个结果都有助于展示团队努力的影响。

对于不遵循流程的团队，他们的建议和潜在节省不会改变，更糟的是，它们会增加。管理层应重申遵循移除闲置资源的流程的重要性，对未应用流程的团队施加压力。FinOps 团队应与未达成目标的团队合作，确定如何协助他们。这在第 24 章中有更详细的介绍。

当避免 $50,000 成本的目标达成后，可以定义新的——并且 hopefully 更雄心勃勃——的目标。

## Conclusion

为了实现目标，每个组织都需要流程，明确规定谁在何时需要做什么。

To summarize:

- 在优化阶段设定目标，并在运营阶段建立流程以实现它们。
- FinOps 成熟度方法适用于流程。不要试图一次实现所有目标。
- 以小步增量开发流程可以让你衡量实现目标的进展。
- 管理层的支持可以帮助团队完成其必需任务。
- 与组织外部的 FinOps 实践者合作，采用在其他组织中已证明有效的流程；考虑加入 FinOps Foundation，使用 FinOps 承包商，或聘请经验丰富的实践者。

有许多流程适合自动化：报告生成和警报是两个主要候选。在下一章中，我们将讨论 FinOps 中的自动化应如何仅遵循明确定义的流程。


