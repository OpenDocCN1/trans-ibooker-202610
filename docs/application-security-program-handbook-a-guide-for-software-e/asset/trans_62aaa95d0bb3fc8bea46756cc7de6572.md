

构建支持软件安全开发的结构，应用安全团队将创建工程组织使用的参考架构、标准和要求。此外，他们成为软件开发结构的一部分至关重要。这意味着他们应参与评审委员会以及其他架构或领导小组，以确保安全成为流程的一部分。最后，应用安全团队应专注于建立教育结构，帮助培养倡导者，或至少提高开发组织的整体安全教育水平。我将在下一章更深入地介绍这一点，但这里的关键要点是，安全必须成为组织文化的组成部分，才能以安全的方式开发软件。建立或扩展安全文化的一种流行方法是采用安全倡导者计划。

## 5.5.1 安全倡导者计划

我在之前的章节中已经谈到过他们。他们被称为很多名字，主要是倡导者或教练，但安全倡导者的目标是将安全知识传播到整个组织。启动一个培养安全倡导者的计划将依赖于组织文化的转变，并且需要工程领导层和整个组织的认可。

安全倡导者的一般定义是非安全员工，通常在组织中担任某种开发或开发支持角色。他们可以来自以下角色之一：

- Developer
- Quality assurance
- Architect
- Designer
- DevOps

要启动安全倡导者计划，应用安全团队首先需要确定哪些团队和应用程序属于该计划。并非每个团队或应用程序都可能参与，尤其是如果他们是小型团队或对组织风险较低。显然，重点将首先放在组织中风险最高的应用程序上。该列表应经过充分审查，并由安全和工程的适当领导批准。这将有助于确保没有遗漏任何团队，并且被有意排除在列表之外的团队已达成一致。

一旦团队列表达成一致，就应定义安全倡导者的角色。最关键的资格是他们应该自愿参与并对安全充满热情。强迫员工加入倡导者计划会导致计划失败。这些倡导者需要花费一定时间推动安全工作。在某些情况下，这可以低至他们所在产品领域安全工作的 5% 时间。然而，组织需要在早期设定时间投入预期，以便不会对优先级和活动产生混淆。安全倡导者的期望因组织而异，取决于其目标。

### Table 5.3 Expectations of a Security Champion

| Sharing security knowledge | Help in making key security decisions | Create and drive security best practices |
| Build threat models | Perform security reviews | Participate in research and development initiatives |
| Participate in bug bounties | Attend security conferences | Prioritize security work in the backlog |
| Monitor for vulnerabilities in the application | Write security test cases as part of the overall application testing |  |

现在期望已经设定并且团队已确定，是时候提名倡导者了。此过程很简单，只需与工程领导层合作确定潜在候选人。一旦确定，就会正式提名候选人。这包括向领导层、其余倡导者团队以及他们所属的工程团队进行沟通。组织还可能让提名的安全倡导者接受正式培训以达到一定的安全知识水平。这在大多数组织中并不少见，可以通过内部或外部培训实现。培训可以设定目标水平或需要完成的认证。一些安全培训平台包括专门为培训倡导者设计的教育路径。如果存在，组织也可以采用利用应用安全团队内部开发的教育内容的方法。

沟通和信息存储也需要定义。根据组织使用的协作工具，这可能就像在 Microsoft Teams、Slack 或 Discord 等协作工具中为倡导者设置一个频道一样简单。此外，倡导者团队会定期开发需要存储以便轻松检索的内容。同样，根据组织的不同，这可能是 Confluence、Hive、SharePoint 或类似的工具。此内容应作为倡导者团队和他们所属的开发团队所有活动的知识库，并包括：

- The champions team charter
- The members of the team and the teams/applications they serve
- Activities and meeting minutes
- Processes for threat modeling, vulnerability management, and performing security reviews
- Training program and opportunities
- External resources

虽然没有人喜欢更多的会议，但定期与倡导者群体进行接触是必要的活动。这可以只是简单的双周或月度会议，用于讨论即将开展的活动、正在进行的项目和当前漏洞。这也有助于保持参与者对倡导者团队的参与度并提高其在整个组织中的可见度。削弱倡导者团队有效性的一种方法是让其松懈或降低团队成员的参与度。

团队确定后，是时候利用它来利用这种安全足迹的增加。现在应用安全团队拥有了构建安全软件的倡导者。这些倡导者嵌入在应用程序中。但这并不是去中心化模型的结束。

## 5.5.2 利用去中心化模型

我们最喜欢的安全倡导者 Dashing Danielle 已被正式邀请加入应用安全团队，因为她在 Superior Product 的旗舰应用程序中展示了出色的安全交付能力。在她正式加入应用安全团队之前，她需要帮助找到她在团队中担任安全倡导者角色的继任者。她向团队宣布了她未来转入应用安全的决定，并询问是否有人有兴趣接任她的倡导者角色。Brilliant Brian 已在团队工作多年，对产品有深入了解。他也参加过许多与 Dashing Danielle 相同的会议，安全是主要议题。他对安全领域越来越感兴趣，并在有机会参与 Superior Products 的安全职能时高兴地举手。

Dashing Danielle 首先与 Brilliant Brian 合作制定培训路径，使他达到倡导者水平。在 Superior Products，他们使用领先的安全工程培训提供商，该提供商包含多个教育模块，旨在培养安全倡导者。Brilliant Brian 被纳入培训计划，第一模块立即开始。这将使他达到平台三个级别中的第一级。一旦他完成全部三个级别并通过每个级别的评估，他将被视为安全倡导者。根据当前进度，Brilliant Brian 将在四个月内完成培训。

在培训进行的同时，Dashing Danielle 向 Brilliant Brian 介绍每个安全倡导者必须熟悉的各种文档和流程。她邀请他参加双周倡导者会议，让他了解对他和其他团队倡导者的期望。他熟悉倡导者团队中的正在进行的项目和主题。

几个月后，Brilliant Brian 完成了培训和评估，他现在是产品安全倡导者双周会议的积极参与者，而 Dashing Danielle 开始在应用安全团队承担更多责任。Brilliant Brian 一直与开发团队密切合作，帮助集成和维护应用安全团队制定的安全扫描工具。他参与了代码评审，以识别不符合应用安全制定的文档实践和参考架构的问题代码模式。他通过鼓励其他成员参加组织提供的培训，提高了团队的安全教育水平。他甚至向当地会议提交了演讲，介绍他在团队中用于将安全带入开发代码的一些技术。在 Dashing Danielle 和领导团队对 Brilliant Brian 处理团队代码安全感到满意后，Dashing Danielle 能够在她的职业生涯中迈出下一步，成为应用安全的正式成员。Brilliant Brian 能够利用 Dashing Danielle 已有的工作，以及应用安全团队持续努力构建开发安全生态系统的工作。从这项努力中

