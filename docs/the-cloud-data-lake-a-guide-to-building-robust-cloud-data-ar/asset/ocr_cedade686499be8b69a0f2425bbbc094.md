+   - **(Must have)** Support data science models on the data lake as measured by a pilot engagement on product offering recommendations to the executive team.
+   - **(Nice to have)** Support more data science models on the data lake as measured by the next set of scenarios on partnership identification for sales and by influencer recommendation for marketing.

# Plan Your Architecture and Deliverables

Once the goals are defined for the data lake, the next step is to define the architecture and the deliverables. As we saw in Chapter 2, a variety of architecture patterns are commonly used, each with its design considerations, as described in “Design Considerations” on page 58. Consider the goals of your data lake along with other factors, such as the organizational maturity, to determine the right architecture for your data lake. Following are some of these common considerations for determining the architecture:

***Cost of the solution***

Identify both the initial cost of setting up as well as the longer-term cost of maintaining your solution, and weigh them against the benefits. A data lake is much cheaper than a data warehouse; however, a data warehouse is easier to get up and running.

***Time taken for the implementation***

In the world of software development, developer and operational time is as important as the dollar bills in terms of cost, so factor in the people and operational aspects into your calculations and get an estimate of the time and effort required for the solution. If you have existing hardware that is reaching end of life, you need to pick a pattern/architecture that can be implemented before the existing hardware runs out of support.

***Backward compatibility***

When you have an existing data infrastructure you need to move to the cloud, it is reasonable to assume that your cloud migration will be done in phases. You need to ensure that while you are moving certain chunks of your solution to the cloud, you can guarantee business continuity without much disruption for your existing applications and consumers. If you have an existing operational database that is supporting dashboards, then ensuring there is compatibility for the existing applications is a consideration you need to factor in.