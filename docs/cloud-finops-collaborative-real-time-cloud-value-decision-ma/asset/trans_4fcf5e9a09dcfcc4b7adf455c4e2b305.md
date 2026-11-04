
## AWS 承诺模型

让我们更仔细地查看 AWS 提供的各种基于承诺的折扣形式。

![](img/4d4736bc293fd892d1da81802d0f9520_290_0.png)

我们将更详细地介绍 AWS 承诺的工作方式，因为许多相同的概念和模式也适用于其他云服务提供商，而目前亚马逊在这一领域拥有最成熟和复杂的体系。

## AWS 预留实例

您为特定区域中的特定资源类型购买 AWS RI。AWS 会在生成账单时根据这些标准将您购买的 RI 优惠券匹配到相关资源。

主要参数如下：

Region
您的资源位于何处（例如 US-West-2 或 EU-West-1）？

Scope
您的预留是按可用区还是按区域？如果按可用区，则将匹配特定可用区的用量（例如 US-West-2A）。如果按区域，则 RI 将适用于该区域内的任何可用区（例如 US-West-2A 或 US-West-2B）。请注意，只有按可用区的 RI 才会获得容量预留，但也可以购买按区域的 RI。

Availability zone
如果为按可用区的 RI，应适用于哪个具体的可用区（例如 US-West-2A）？

Instance characteristics
实例特征
Type
由实例族（例如 m5、c4、t3）和大小组成。实例大小因族而异，但通常具有值如 large、xlarge、2xlarge 等。
Tenancy
确定资源是共享还是专用。
Operating system
指定实例包含的操作系统和软件许可（Red Hat Enterprise Linux、Microsoft Windows 等）。

如果您的 RI 参数与您正在运行的用量不完全匹配，则 RI 不会应用折扣。因此，确保以正确的参数购买预留至关重要。

AWS 会以非确定性的方式将折扣应用于随机选择的资源，就像优惠券应用于相关资源一样。虽然 AWS 会从最小实例到最大实例进行应用，但目前没有办法确保折扣应用于特定资源而不是其他匹配的资源。

## 成员账户亲和性

客户影响 RI 应用方式的一种方法是使用成员账户亲和性。AWS Organizations 提供名为合并计费的功能，允许账户成为成员并将成本合并到单个管理账户中（以前称为 payer account）。在 AWS Organization 中只能有一个管理账户，且账户只能是某个 AWS Organization 的成员。当然，出于各种原因，您的企业中可能存在多个管理账户。

当您购买 RI 时，可以在管理账户或任何成员账户中进行此购买。RI 可以在同一 AWS Organization 内的账户之间共享其提供的折扣。然而，它们对所属的本地账户具有亲和性，这意味着在特定账户中购买的 RI 将首先提供给该账户中的资源。对于我们稍后讨论的 SP 也是同样的方式，但在本节中我们仅使用 RI 来描述账户亲和性的工作原理。

如果本地账户没有 RI 的匹配用量，则该预留将对同一 AWS Organization 中任何其他 AWS 账户的匹配用量应用折扣。

RI 共享可以按每个 AWS 账户禁用，但默认情况下是开启的。如果禁用 RI 共享，该账户内的任何 RI 都不会在账户外应用折扣，并且当 RI 与账户内的任何用量不匹配时，不会应用任何折扣，在 FinOps 中我们称之为空置或预留浪费。禁用共享有效地将 RI 固定到成员账户，如果出于预算目的必须不共享的话。考虑在禁用共享时可能会导致浪费，因为未使用时折扣不会共享给他人。这似乎没有意义，通常确实如此，但在资金仅授权用于特定目的的情况下可能需要，例如在政府机构中资金的使用受到严格限制。此外，同一 AWS Organization 中其他账户的 RI 不会对已禁用 RI 共享的任何账户应用折扣。

人们通常对 RI 共享的工作方式存在误解。为了确保您理解正确，请查看图 17-2 中的示例。

![](img/4d4736bc293fd892d1da81802d0f9520_292_0.png)

Figure 17-2. Discount interactions with RI sharing
图 17-2. RI 共享的折扣交互

Figure 17-2 shows a typical account structure: a management account with a handful of linked member accounts. You can see in the diagram there are three RI purchases: in the management account, in member account A, and in member account C. Account affinity will cause the RIs within the account to apply a discount to the local account first. If no usage matches the RI within the same account, then RIs may apply a discount to usage in another member account via RI sharing.
图 17-2 显示了一个典型的账户结构：一个管理账户和一些关联的成员账户。您可以在图中看到有三次 RI 购买：管理账户中、成员账户 A 中以及成员账户 C 中。账户亲和性会导致账户内的 RI 首先对本地账户应用折扣。如果同一账户内没有匹配的 RI 用量，则 RI 可通过 RI 共享对另一个成员账户的用量应用折扣。

In the example layout in Figure 17-2, the following may occur:
在图 17-2 的示例布局中，可能出现以下情况：

- RIs in the management account can apply to account A or B because they both have RI sharing enabled.
- 管理账户中的 RI 可以应用于账户 A 或 B，因为它们都启用了 RI 共享。
- Account A can benefit both from the RIs purchased within the account and from RIs in the management account but will have preference for the RIs within the account.
- 账户 A 既可以从账户内购买的 RI 中受益，也可以从管理账户的 RI 中受益，但会优先使用账户内的 RI。
- Account B can benefit from RIs from both account A and the management account.
- 账户 B 可以从账户 A 和管理账户的 RI 中受益。
- Because account C has RI sharing disabled, the RIs within that account will not apply a discount outside of account C and will go unused when no usage matches.
- 由于账户 C 已禁用 RI 共享，该账户内的 RI 不会在账户 C 外应用折扣，并且在没有匹配用量时会未被使用。
- Account D will not receive any RI discounts because it has RI sharing disabled and does not have any RIs purchased within the account.
- 账户 D 不会收到任何 RI 折扣，因为它已禁用 RI 共享并且账户内未购买任何 RI。

It’s worth taking a moment to reread this list. The majority of people we’ve met over the years commonly misunderstand some part of RIs, and this can lead to RIs not applying discounts where you expect them to. Knowing how to control where RIs apply and where they don’t allows you to choose the best strategy for where to buy reservations.
值得花点时间重新阅读此列表。我们多年来遇到的大多数人通常都会误解 RI 的某些部分，这可能导致 RI 未在您期望的地方应用折扣。了解如何控制 RI 应用的位置以及不应用的位置，可以让您选择最佳的购买预留策略。

## 标准与可转换 RI

当您从 AWS 购买 RI 时，可以作为单个租约的一部分同时购买多个 RI。标准预留实例 (SRI) 允许您在最初购买时设置参数。

对于 SRI，可以修改其中一些参数，例如可用区和范围（区域与可用区）。对于 Linux/UNIX RI，同一 RI 租约的一部分可以拆分和合并（例如，两个 medium 合并为一个 large）。然而，您无法修改许多其他参数，例如期限长度、操作系统、实例族（m5、c5）和租用类型（共享或专用）。一旦购买了标准 RI，您就承诺在 RI 期限（一年或三年）内运行与这些参数匹配的资源。

对于 EC2 实例，有一种更灵活的选择，即可转换预留实例 (CRI)。作为折扣百分比较低的回报，您可以在期限内将 RI 预留交换为不同的预留。交换仍有一些限制适用。

无论如何，您都不能减少对 AWS 的整体承诺。例如，如果交换导致承诺降低，您就不能用更便宜的 RI 预留替换昂贵的 RI 预留。在这种情况下，需要进行补差费用，使修改包含更多 RI 或更昂贵的参数。当 RI 交换导致费用增加时，AWS 将向您收取任何应付预付款的净差额。

有一个流程允许您拆分 CRI 预留，以便您可以转换单个 RI 租约内部分 RI 而不是全部。同样，也有流程可以将多个 CRI 预留合并在一起，允许您将它们全部修改为一个新的 CRI 预留。然而，在期限内无法修改 CRI 的某些参数：

- CRI leases are purchased for a particular region and cannot be exchanged for another region.
- CRI 租约是为特定区域购买的，不能交换到另一个区域。
- CRI leases are for EC2 instances only and cannot be converted to other services; for example, if you move your database from an EC2 instance into RDS, you cannot exchange your CRI for an RDS RI.
- CRI 租约仅适用于 EC2 实例，不能转换为其他服务；例如，如果您将数据库从 EC2 实例迁移到 RDS，则不能用 RDS RI 交换您的 CRI。
- The term for CRIs cannot change, except that when joining CRIs you can sometimes extend the term from one to three years.
- CRI 的期限不能更改，除非在合并 CRI 时有时可以将期限从一年延长到三年。

除了这些限制外，总体而言 CRI 对组织的风险要低得多。除非您可能在期限内减少 EC2 的总使用量，否则您可以在期限内更改 EC2 使用参数并交换您的 CRI 以匹配您的参数的预留。请记住，然而，分析
