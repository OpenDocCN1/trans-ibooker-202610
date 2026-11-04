# Shared costs across accounts

A common problem that FinOps practitioners face is how to allocate central costs in the cloud world. You will need to split and spread out costs like volume discounts, enterprise support, and RI prepayments. Inside accounts shared costs for items including Kubernetes infrastructure, networking costs, monitoring, and logging systems will also need to be handled by FinOps. For more on the ways to split out shared costs, see the open source playbook on *FinOps.org*.

Returning to the story shared by Ally, in the early days, when they were using the multitenant account approach, tagging coverage was not as good and left most of their spend untagged. So Ally focused on reducing the ratio of spend to untagged spend over time. On an account-by-account basis, team leaders in charge of particular products have been given a granular view into the services that comprise those products. Near-real-time reporting is now provided, resulting in increased visibility into spend over the month and quarter. This has helped to close allocation gaps by shining a light on them.

Following a typical maturity curve in FinOps, Ally began by keeping cloud volume discounts, enterprise support charges, and prepayments like RI amortization out of the reporting that end teams received. Over the years, Ally’s FinOps practice has matured, and her teams have factored more and more of those costs into their spend reporting. Today, her teams see the properly adjusted prices. They are able to look at their fully adjusted spending, with amortizations and accurate volume rates baked into the numbers, which gives them a more accurate picture of actual cost.

# The Showback Model in Action

Essential to showback is clearly tying costs back so teams know how much they’ve spent, showing each team their actual spend against forecasted spend, including the variance between the two and a comparison to what was budgeted. Provide showback reporting on a weekly, monthly, and quarterly basis, as well as providing regular, more granular reporting into cost drivers and anomalies.

Aim to have your CTO run a monthly operating review with each of the engineering VPs to show their spend. When there aren’t any glaring variances, these meetings tend to be quite brief. Larger variances bring a more focused approach. The FinOps processes should provide VPs an as-early-as-possible heads-up that they will overshoot their budget.

When a variance comes to light, quickly highlight the issues and the cost drivers that caused the variance, and then the VP can work with the team(s) to implement a fix without waiting for the monthly operating review. This is a great example of the breaking down of silos and of real-time decision making.