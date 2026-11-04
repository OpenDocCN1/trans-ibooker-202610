
growth of the cloud spend returns. If a forecast was generated in 2022-04 without factoring in the expected impacts of introducing FinOps, the forecast would have been generated too high. If instead the forecast was generated in 2022-07, the downward trend would have impacted the forecast and the natural growth in cloud spending that is hidden under the cost reductions at the time; if they had not been accounted for, the forecast will be generated too low.

![](img/4d4736bc293fd892d1da81802d0f9520_227_0.png)

Figure 13-5. Sample cloud spend with FinOps activities starting in 2022-05

## Stories from the Cloud—Drew Lowe

Drew Lowe leads the FinOps sourcing team for Hulu Disney, partnered with Anthony Logan. At the beginning his team had no mandate from the top, which allowed them to focus on policies. Financial planning and analysis (FP&A) was lacking technical depth, while engineers were not concerned with cloud cost. Hulu is primarily using AWS with some workloads in data centers. Drew’s team ultimately reports to the CFO.

Forecasting reached a turning point when engineers asked for 4.5 times the previous year’s spend for the next budget cycle. The decentralized engineering teams made pricing decisions on products without understanding the underlying costs. The company’s objectives and key results (OKRs) put the viewer first. While Drew was able to connect spend to revenue, he was concerned about overspending.

A key OKR around accuracy to forecast was added for all engineers. Drew’s team started with good data and made sure they could trust the numbers. By driving visibility, he was able to build demand-driven forecasting, which in turn allowed them to optimize spend. By identifying the top-spending teams and learning about their workloads, his team was able to get the majority of the forecasts in line with expectations.

Drew is using a hybrid forecasting model, where he uses business driver-based forecasts for top services and trend-based forecasting for the long tail. This allows him to update forecasts every month with an average variance of about 2.5% and the worst case being about 9% variance. To get there, Drew decided to forecast by workloads instead of cloud stock keeping units (SKUs). However, engineering volatility is challenging, for example, predicting load testing or new services. For new services Drew uses swag estimates to track these special projects on their roadmap.

Drew’s takeaway: “A tagging strategy is imperative. Partnering closely with the highest cost workloads works very well. Start building relationships with CTO and CFO, then engage with the engineering leads.”

## Forecast and Budgeting

Forecasting and budgets go hand in hand. We will cover the stages of budget generation soon, but it’s important to understand that using forecasts to set budgets is one of the main contributors to having budgets that reflect reality.

If you’re trending off of your forecast, you must ask yourself: does something need to be changed to bring you back in line with the forecast, or do you need to update your forecast? Therefore, you should track your actual spend versus your forecast every day. Graphs for cloud spend should include your forecast to determine if there are any actions you need to take (see Figure 13-6). Then you break down the information to the team level, which allows you to see whose actions, if any, are pushing you out of your forecast. There may be business reasons (compliance, performance, etc.) that mean you should make the choice to simply adjust your forecast.

![](img/4d4736bc293fd892d1da81802d0f9520_228_0.png)

Figure 13-6. A single graph showing the six-month forecast, projected budgets, and actual budgets/use for the previous six months

The general consensus in the FinOps community is that cloud team forecasts should be done on a rolling 3- to 12-month basis. Atlassian does them in 3-month increments, but also projects forward 3 years to explore where they think they will be. A large media company we’ve worked with has a variation on this: a yearly forecast, broken into monthly targets that are revised every quarter.

Evaluating forecasts on a quarterly or monthly basis often isn’t frequent enough. Skilled FinOps practitioners constantly evaluate variances based on live data (sometimes daily) to see which teams are deviating from quarterly forecasts and then lean in with them quickly. This lets them get in front of variances before they grow larger. When actuals vary from forecast and a decision is made to maintain this increase in spend, it is reported up the management chain the day a decision is made. Conversations can take place between management and the business to determine the value, if any, that the increased spend is generating and any remediations necessary.

These constant evaluations help the business make better decisions as the month progresses, while showing the soonest point the spend can be bent downward. Budgets shouldn’t be meant to prevent spending, but rather provide a vehicle for identifying when spending deviates from plan—which, remember, enables conversations.

## The Importance of Managing Teams to Budgets

It’s easy to think of budgets as restrictive controls with harsh limits. However, budgets need not, and should not, restrict innovation. As we’ve mentioned before (and it’s worth repeating!), FinOps isn’t about saving money—it’s about making money. In FinOps, budgets are about focusing your investment to fuel innovation instead of preventing it.

Initial conversations with engineering teams about cost optimization typically aren’t productive. They haven’t traditionally had to consider cost, and they are under constant pressure to deliver more and deliver it faster. So they are understandably resistant to spending their precious cycles optimizing for cost—often seen as someone else’s job.

When a FinOps practitioner sits down with the architecture team to review findings on cost, the practitioner often encounters pushback on advice and only sometimes sees the suggested changes implemented.

However, if teams are given specific cost targets or goals to hit, they tend to hit them. Cost-savings advice from management is welcomed with open arms. Dev teams lean in, actively participating and asking for ways to cut their costs while maintaining speed and quality. Rightsizing happens because the teams have clear goals. A leadership-driven culture of cost optimization will recruit them as allies and give visibility into how they can help the business. They begin to understand that FinOps is not about cutting resources but about enabling cost-conscious speed and innovation.

For many tech unicorns, budgeting is a four-letter word. It’s a necessary evil that distracts from trying to gather up as much market share as possible in their highly competitive growth markets. They’re immersed in a never-ending arms race to deliver bigger and better features, engaging content, or data-science-driven recommendations. There’s an unfounded premise that budgeting slows it all down.

But for most enterprises, budgeting is a necessary step in launching a project. No green light is given until a budget is approved. The cloud has drastically changed the budgeting process. Gone are the days when a budget could be controlled by the power of a purchase order, which prevented paying anything beyond what was planned. Because of the distributed and lightly governed nature of cloud spend, most organizations—big and small—struggle to accurately budget in the public cloud.

Like most things, putting effective budgeting in place is a journey. Here are four levels of budgeting that organizations typically go through:

### Level 1: No one is budgeting at all

During a recent meeting with a well-known tech unicorn, the team was asked how they defined budgets for their cloud spend. The answer was a shaking head: “We don’t have budgets. We don’t want to constrict engineers.” This very same company had also realized they were spending considerably more than they wanted to. The organization had made a plan for how much they were going to spend in cloud for each of the next three years, but because it wasn’t carried down to the individual teams responsible for consuming cloud resources, they were already well over what they and their cloud provider had agreed to. In this case they ignored the budget totally, not even practicing informed ignoring.

### Level 2: Spend what you did last year

The budgeting process at a well-known retailer was as simple as trying to stay within what the company had spent the previous year—taking a static forecast and using it to set budgets. Though on the surface this seemed like a good starting point, it wasn’t based on an analysis of whether what the company was spending was the right amount for what it was doing. Very much like government spending, it created the wrong incentives by encouraging teams to spend all of their budget so they wouldn’t lose it the next year.

This type of budgeting—a “first stop the bleeding” approach—is actually fairly effective at halting out-of-control cloud spend. But it doesn’t result in savings or any meaningful measure of cloud spend tied to business objectives. It’s akin to how budgeting was done in the old world of CapEx-heavy data center purchasing. You looked at what you had then and how much capacity you had left, but you had no insight into the actual efficiency of what you were using.
