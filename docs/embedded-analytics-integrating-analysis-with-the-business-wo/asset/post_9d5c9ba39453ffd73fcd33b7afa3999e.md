
This is where the capabilities of the tool you're using to build analytics objects really come into play. Many data visualization tools are highly customizable and allow the user to build interactive experiences that are more akin to “dynamic infographics”: very tightly integrated into the target system, augmented with narrative and imagery, and behaving more like an application than a traditional dashboard.

For inspiration, see Tableau's [public website](), which showcases some of the best examples of dynamic and interactive visualizations created using the tool.

## Putting It All Together

We’ve talked about the types of analytics objects you can embed and how capabilities and value can be added by considering some of the human and technical aspects of embedded analytics solutions. Now let’s think about what we’ve learned in relation to some real-world use cases.

## Embedding Workflow

A core principle of embedded analytics is keeping users “in the flow” when making decisions informed by data. The fact that the analytics object is *in* the tool you’re already using means you don’t have to *leave* the tool, which certainly helps. But as we’ve discussed, the nature of the visualization, how fast it reacts, and how effective it is for the user are all significant factors.

This means it’s important to think about the context of that workflow. Where has the user been before they get to see the embedded content? What will they do once they’ve interacted with it? How can the embedded objects drive upstream and downstream processes that might be integral to the “flow” we keep talking about?

## A Typical Reporting Automation Workflow

Let’s consider an example of a basic reporting workflow. A finance team needs to gather data to provide a monthly report and requires the involvement of the IT/database team to do so. Once they’ve got the data they need, they create a document (or update a preexisting one), and provide that to the management team to use as a decision-making tool.

What do we get with this process? Delays while different teams are involved, repetition of effort, potential for error, unmanaged proliferation of content, and potential security problems, too. This is a suboptimal pattern that’s probably happening in thousands of organizations right now.

Figure 5-13 illustrates this convoluted, multidependency process as it might apply in a typical organization. This is often the first real-world use case that gets addressed with embedded analytics.

![](img/900d7b17f0065e515479188270e9709d_97_0.png)

Figure 5-13. An example of a resource-intensive and costly analytics workflow

When we incorporate an embedded analytics workflow, as shown in Figure 5-14, the analytics content is integrated into an organization-wide “portal,” where analytics objects are driven by data from the organization’s finance database and only accessible to the management team. A given manager need only log in, review, and interact with the analytics object (a dashboard, for example) to make an informed decision.

![](img/900d7b17f0065e515479188270e9709d_97_1.png)

Figure 5-14. Using embedded analytics to accelerate time to insight

The contrast is obvious; there is a single report, which doesn’t need to be updated by anybody. The data is accurate, access to it is tightly controlled, and the reporting content is immediately available. In fact, the finance and IT/database teams are not involved at all.

The potential time and cost savings realized by using embedded analytics in this way are nontrivial.

## Using Embedded Analytics to Power Prescriptive Analytics

*Prescriptive analytics* is the term used to describe an analytics model where the solution specifically describes the actions needed to achieve a particular outcome. It sounds simple but often involves a lot of complexity, not least because most prescriptive analytics techniques involve presenting nontechnical users with the results of machine learning processes, which is not an easy feat to accomplish.

When embedded analytics solutions are implemented successfully, it becomes possible to integrate prescriptive analytics processes to provide a seamless experience for end users. Here’s an example:

- 1. A sales manager accesses an embedded chart in their CRM system indicating that their sales figures are surprisingly low for the current quarter. They contemplate addressing the problem with the sales team.
- 2. A machine learning model, which has been trained on historical sales figures over the last five years, determines that to meet the current targets the best option would be to focus efforts on reducing the prices of three specific product categories that have historically generated strong sales, with appropriate discounts applied.
- 3. The specific discount percentages are determined for each product category, and the recommendation is shown alongside the embedded chart in the CRM system as the optimal way to address the poor sales performance.

This is truly closing the loop. Rather than the team addressing the issue by using intuition and guesswork, the sales manager can issue a single, statistically supported directive: *discount these three product lines and we’ll be back in business*.

**NOTE**

It's worth noting that prescriptive analytics is rarely deployed effectively due to the complexity of the processes involved and the requirement for advanced analytics and data science and machine learning (DSML), the use of which often requires highly skilled data scientists or analytics engineers. As ever, software is increasingly democratizing this discipline.

We're going to spend more time talking about DSML, automation, and augmentation as they apply to embedded analytics solutions in Chapter 9, but suffice it to say that DSML techniques are often at the center of the prescriptive analytics features we’ve discussed.

## Operationalization (or “Write-Back”) of Data

In many cases, the workflows that we’ve described so far may include steps where data needs to be written back into the data source or other operational system to drive further processes. Here are three real-world examples:

- A user-tracking dashboard shows that a given user has opened a marketing email. The user’s record within the CRM system therefore needs to be updated to show that the user has responded to the marketing campaign, which removes them from the recipient list for a period.
- An executive adds a comment about an outlying data point in a chart, which needs to be displayed as a tool tip against that data point for anyone else viewing the chart in the future.
- A user interacts with a dashboard showing errors in a production line. When the user clicks a specific error, a ticket needs to be created in an error-tracking application, containing details about the time and nature of the error.

At a high level, all these examples work in a similar way. Data derived from the embedded *analytics solution* (by the input of the user or derived purely from the data itself) is written back to the *target system* (either a data store within a CRM/ERP system or a dedicated database). In both cases, this achieves the goal of “closing the loop” with an analytics workflow: you use analytics to derive insight and update the data accordingly in a single workflow.

These examples all require some level of integration between the analytics solution and the target system(s), but the key point is that operationalization of data can often be driven from within the embedded analytics solution itself, ensuring that the user can *execute* an actionable insight without leaving the solution.

Operationalization of data as part of an analytics workflow is a highly innovative and heavily invested area for both analytics and data warehousing vendors. For those familiar with OLAP versus online transaction processing (OLTP), the technical distinction between transactional use cases and analytical use cases has historically necessitated different technical approaches and, therefore, different products.

> **NOTE**
> 
> OLTP use cases usually indicate small volumes of data *written* into a database very frequently, whereas OLAP use cases usually indicate large volumes of data *read* from a database less frequently.

With the advent of scalable SaaS offerings and advances in data storage, caching, and query performance, this distinction is becoming less prevalent. As of the time of writing, some data platform vendors (Snowflake, for example) are working on unified services to provide databases that can address OLAP and OLTP workflows simultaneously: a single store of data that your ERP system writes to *and* that acts as the source of truth your analytics application reads from.

## Business Case Integrations
