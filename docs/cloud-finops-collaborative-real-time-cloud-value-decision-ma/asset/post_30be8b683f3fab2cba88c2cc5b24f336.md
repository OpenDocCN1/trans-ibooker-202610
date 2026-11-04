
Table 11-2. Cost allocation example with amortized support charge

| Business units | Allocation amount | Allocation percentage |
|---|---|---|
| Sales operations | $55K | 50% |
| Engineering—QA | $33K | 30% |
| Engineering | $22K | 20% |
| Total | $110K | 100% |

Sharing the enterprise support charge, you can see that:

- Sales operations will be accountable for a total of $55,000 ($50,000 direct cost + $5,000 support charge allocation).
- Engineering—QA will be accountable for a total of $33,000 ($30,000 direct cost + $3,000 support charge allocation).
- Engineering will be accountable for a total of $22,000 ($20,000 direct cost + $2,000 support charge allocation).
- The enterprise support charge has been distributed and does not impact a central budget.

For more information on managing shared costs, the open source playbook on the FinOps Foundation website expands on this topic.

## Chargeback Versus Showback

There are ongoing conversations in the FinOps community about when to do chargeback or showback.

Both approaches drive accountability, but you need to look at the core difference between them. Chargeback happens when the actual cost being incurred is allocated back to the budget or profit and loss (P&L) of an individual team. Showback happens when you show teams what they are spending, but the dollars are allocated internally from a central budget.

The best way we’ve seen to think about the difference comes from an article by Barry Whittle:

> Showback leaves a bill on the table for analysis; chargeback leaves the same bill, but requires payment.
> —Barry Whittle, Apptio

In practice, the choice between the two is more often informed by the accounting requirements of the organization rather than by its maturity. Enterprises will often choose one or the other due to an organizational bent toward or away from departmental P&L statements. For instance, a finance leader or CFO may be against the concept of P&L across the various parts of the enterprise, or it may not make sense for certain parts of the business. In these cases, organizations will lean toward showback and not do full chargeback. There are often requirements to have separate accounting of costs for different business units, or different parts of the enterprise operating under different taxing authorities. These types of situations may require chargeback to occur.

But chargeback at too low a level can impose other difficulties too. It is common in the cloud to have adjustments, or credits, that appear after the close of the accounting period. Indeed, all usage is reported in arrears, so the need to regularly estimate end of period costs and make later adjustments can create difficulties for some finance and accounting groups. Angry accountants do not make good FinOps partners.

## Stories from the Cloud—Rob Martin

The transition from showback to chargeback takes some time. Rob Martin, Director of Learning at the FinOps Foundation (he was formerly on the AWS Cloud Economics team) shared this story:

> If there’s not a burning need to do chargeback for legal or tax reasons, the transition can take years to accomplish. At a large company I supported, it took three fiscal years to get to true chargeback. Their first year of cloud use was very exploratory: they spent most of it learning about how cloud worked, experimenting with their first applications, and building an understanding of just how different cloud budgeting was from their traditional model. Very little was done in budgeting or forecasting during year one because the organizational IT budget was already set, and the vast majority of their IT was still on premises. In their second year, the teams using cloud got a bit more flexibility but were still held to the traditional budgets. The FinOps team provided showback to each application team, but they were being funded from large traditional budget line items. In year three, the organization was able to finally clearly define how the cloud teams would control and flexibly manage their budgets throughout the year, and the finance team was confident that the P&L owners over those teams could be charged directly for their costs in the accounting system. The teams forecasting spend were confident in these forecasts because they’d had access to showback reporting over this entire time and had built the processes that would make them successful when the costs actually hit their accounting line items.

## A Combination of Models Fit for Purpose

Ally Anderson shared that her company uses a combination of chargeback and showback. When it’s a customer-facing SaaS workload, the company does a showback spend while centrally managing the budget. Then, for certain R&D workloads, they do a chargeback to the specific team’s P&L.

Ally confirmed that allocating spend among teams is complex and ongoing. She maps each team’s dozens of disparate accounts against the relevant products and cost centers that the business needs to report against. However, after a big reorg, she told us that teams rarely tear down cloud resources and rebuild them in different accounts to match the new organizational structure. But it’s an important step in a healthy FinOps practice. You must take existing usage of cloud resources and reattribute them to the appropriate team, portfolio, VP, or cost center.

It’s a common challenge that many FinOps teams encounter. The way engineers tag resources doesn’t necessarily align to the way the business needs to report cloud spend. And spending often needs to be remapped to match the cost centers or products that are the rightful *owners* of cloud spend.

## Accounts, Tagging, Account Organization Hierarchies

Each of the major cloud service providers have a range of elements that, when used together, enable you to build a successful cloud cost allocation strategy:

### Account hierarchies

How your engineers place their infrastructure into AWS accounts, Google Cloud projects, and Azure subscriptions forms the first layer of your cost allocation strategy. Individual accounts can be used to isolate costs of one environment or application from another.

### Account naming

Often overlooked in the early stages of an organization’s cloud journey is having an account naming standard to record details about the account’s use. Aim to include details like the product and environment that will be operating inside the account. An example name might be “productA-production-01.” The FinOps practitioner should encourage the organization to have a central checkpoint to ensure this naming process is followed. All requests for accounts should go via a central process. In AWS, this would take the form of linked accounts or cost categories. In Google Cloud, this would be in the form of projects or folders. In Azure, it might be subscriptions or resource groups.

### Tagging

Make heavy use of tagging and labeling, especially in accounts that have multiple applications or business units operating inside them. A tag key that tracks the department or cost center for the cloud resources is especially helpful for FinOps. Tags help carve out spending inside accounts into different groups. Require that tags be applied to all resources, using various levels of proactive enforcement. However, you should know that not all spending is taggable and not all end users are compliant. So there is often still a sizable chunk of untagged spend that is reported out to management as a data hygiene metric.

### Shared costs across accounts

A common problem that FinOps practitioners face is how to allocate central costs in the cloud world. You will need to split and spread out costs like volume discounts, enterprise support, and RI prepayments. Inside accounts shared costs for items including Kubernetes infrastructure, networking costs, monitoring, and logging systems will also need to be handled by FinOps. For more on the ways to split out shared costs, see the open source playbook on *FinOps.org*.

Returning to the story shared by Ally, in the early days, when they were using the multitenant account approach, tagging coverage was not as good and left most of their spend untagged. So Ally focused on reducing the ratio of spend to untagged spend over time. On an account-by-account basis, team leaders in charge of particular products have been given a granular view into the services that comprise those products. Near-real-time reporting is now provided, resulting in increased visibility into spend over the month and quarter. This has helped to close allocation gaps by shining a light on them.

Following a typical maturity curve in FinOps, Ally began by keeping cloud volume discounts, enterprise support charges, and prepayments like RI amortization out of the reporting that end teams received. Over the years, Ally’s FinOps practice has matured, and her teams have factored more and more of those costs into their spend reporting. Today, her teams see the properly adjusted prices. They are able to look at their fully adjusted spending, with amortizations and accurate volume rates baked into the numbers, which gives them a more accurate picture of actual cost.

## The Showback Model in Action

Essential to showback is clearly tying costs back so teams know how much they’ve spent, showing each team their actual spend against forecasted spend, including the variance between the two and a comparison to what was budgeted. Provide showback reporting on a weekly, monthly, and quarterly basis, as well as providing regular, more granular reporting into cost drivers and anomalies.

Aim to have your CTO run a monthly operating review with each of the engineering VPs to show their spend. When there aren’t any glaring variances, these meetings tend to be quite brief. Larger variances bring a more focused approach. The FinOps processes should provide VPs an as-early-as-possible heads-up that they will overshoot their budget.

When a variance comes to light, quickly highlight the issues and the cost drivers that caused the variance, and then the VP can work with the team(s) to implement a fix without waiting for the monthly operating review. This is a great example of the breaking down of silos and of real-time decision making.
