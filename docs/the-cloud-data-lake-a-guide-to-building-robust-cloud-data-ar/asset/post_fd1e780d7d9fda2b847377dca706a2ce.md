
# Table 7-1. Cloud data lake implementation considerations depending on the scale of your organization

| Area of investment | Reference | Small-scale data lake (largely controlled by the data platform team end to end) | Large-scale data lake (data platform team is running into limits or the use cases are highly variegated and hard for the data team to keep track of) |
|---|---|---|---|
| Data organization | "Organizing Data in Your Data Lake" on page 72 | Organize your cloud data lake storage into zones using the storage organization mechanisms. | Organize your cloud data lake storage into zones. Organize your compute processing clusters in a way that segments critical data pipelines from noncritical exploratory data usage. |
| Data governance: data classification | "Data Classification" on page 81 | Manual or semiautomated would suffice; plan for automated classification. | Automated data classification is a hard requirement; the manual or semiautomated classification does not scale. |
| Data governance: data discovery | "Metadata Management, Data Catalog, and Data Sharing" on page 82 | Publish the metadata in a metastore or a data catalog offering that is not too expensive. Data sharing is useful if you share your datasets with other organizations. | Invest in a self-serve data catalog that can be used by data producers; data sharing will largely help with the self-serve approach. |
| Data governance: data access management | "Data Access Management" on page 83 | Implement storage-, compute-, and network-level access policies. Data-access-policy tools are nice to have but not a hard requirement. | Data access tools at the cross-component level are a worthy investment that reduces the load on the data team. Implement complex policies based on the data classification scales to meet compliance requirements. In some cases, you can lock down access at the compute, storage, and network levels to the bare minimum and leverage these data access policies for end-to-end data management. |
| Data governance: data quality and observability | "Data Quality and Observability" on page 85 | Define SLAs and SLOs for your critical datasets and jobs; monitoring could be semi-automated to start. | Invest in data quality and data observability solutions to be able to scale to the diverse use cases running on your data lake. |
| Data lake cost management | "Manage Data Lake Costs" on page 89 | Understand every line in your cloud data lake bill to avoid surprises. Implement data retention policies and tiered storage to optimize data lake storage costs. | In addition to the small-scale data lake practices, implement quotas for consumption across your departments to ensure there are no surprises. |

At the end of Phase 3, the goal is to have a timely launch with a complete set of features, while also ensuring that the data platform team is set up for smooth operation of the data lake.

## Phase 4 of Decision Framework: Operationalize

When you build and ship software, whether a website or a mobile application or a software application, the primary focus is on code, and any change in requirements and functionality can be accomplished by changing the code. However, in the development of data, changes in code, data, or configurations can alter the behavior of the consumption patterns.

Operationalizing the data lake involves the following components:

### Managing new requirements or change requests

You can almost be certain that the number of use cases that run on your data lake will continue to grow, and it’s only a matter of time before you will get new requests. You can use a process similar to Phase 1 to triage and debug the requirements.

### Troubleshooting and debugging

As with a software product, ensure that you have a process and workflow for incident management. It is a best practice to have an on-call support system for your data platform similar to support for a software system. The SLAs, SLOs, and SLIs you set for your data platform are critical to prioritizing the level of engagement for customer issues. I also recommend that you track the various incidents and the associated root causes to ensure that you can identify the areas where technical debt is built and address them systematically. These could involve performance tuning or identifying scale bottlenecks, among other areas. Chapter 4 and Chapter 5 provide more context and information on the approaches to tuning your cloud data lake for scale and performance.

## Summary

In the first six chapters, I went through the various aspects of designing, building, and implementing a cloud data lake solution. In this chapter, I have put all of these learnings together to show you how to apply these concepts as you build your cloud data lake solution. I introduced a four-step framework of assess, define, implement, and operationalize in describing these concepts. You can use this framework as is or customize it as you see fit. My strong recommendation is that you take the time to assess your business and customer requirements and use those to guide your design and implementation. The due diligence you do in earlier phases ensures a smoother operation. Finally, expect new discoveries at every stage, so go back to the output of your previous phases and tune it as you see fit.

## CHAPTER 8

## Six Lessons for a Data Informed Future

> *It’s amazing how a little tomorrow can make up for a whole lot of yesterday.*
> 
> —John Guare

The promise of a cloud data lake architecture lies in the boundless diversity of scenarios that it enables. In the previous chapters, we’ve focused on the most commonly used patterns of data processing with Spark- and Hadoop-flavored technologies. Other aspects—such as real-time stream processing, which generates quick insights on real-time data, and advanced analytics scenarios, which build smart applications on the data lake—are gaining fast adoption. One thing that all the concepts and frameworks we covered in the previous chapters have in common is that at every juncture in the design or implementation of the cloud data lake, choices are available to you, and each choice comes with trade-offs on cost, complexity, and flexibility. As you make these decisions when designing your cloud data lake, it’s only natural to have the following questions:

- How do I know I made the right choice?
- As my organization grows and so do the scenarios on my data lake, how do I iterate and drive transformation?
- How do I ensure that my organization can be agile to gather and address the next set of requirements?
- How do I think about a global strategy and stay ahead of the needs of my organization?

In this chapter, I leverage the format of lessons learned to provide a structure for you to think about the technical, cultural, and organizational decisions that will help you realize the value of data for your organization.

The journey of a thousand miles begins with a single step. Once you take that first step, it is only a matter of time before you see yourself sprinting ahead.

## Lesson 2: With Great Power Comes Great Responsibility—Data Is No Exception

With the great power and diversity of scenarios that data brings to your organization, one important aspect for you to consider is managing the data end to end in a responsible fashion, adhering to ethical business practices and ensuring fairness and equitable experiences for your customers.

Let’s see this with an example. With the data strategy and a cloud data lake in place, Klodars Corporation leverages social media trends and correlates them with its sales and customer data to understand the consumption patterns. It identifies that customers between ages 30 and 45 tend to click on campaigns and ads in social media to buy umbrellas and rain gear from its websites. This helps its marketing department target campaigns and offer discounts and promotions to customers of this age group. While this helps the business, Klodars inadvertently optimizes its business for customers in this age group, potentially having the side effect of marginalizing older customers. One of the reasons older customers did not buy from the website could be because they were not using social media as actively as the other demographics, which ended up as a disadvantage for the company. In the longer term, this also affects the business because once it saturates the market for ages 30–45, Klodars doesn’t have a way to expand.

Although this is an example I crafted to illustrate the issue of having a responsible data strategy, it is not far from reality. According to the Washington Post article “Racial Bias in a Medical Algorithm Favors White Patients Over Sicker Black Patients,” a health services provider company named Optum leveraged data to understand what segment of patients would benefit from extra medical care. Unfortunately, the company is now under investigation because its algorithm allegedly undermined the health needs of Black patients due to the bias in the datasets toward a certain segment of our population.

To ensure that you start with the right set of governance, security, and ethical practices in your cloud data lake strategy, here are some of the practices you can put in place:

Include and consult the experts in your organization to proactively identify data risks

You most likely have a regulatory body in your organization for other parts of your business; this could be your legal department, compliance organization, or any regulatory affairs department. Involve them early, ensure that you understand the risks and best practices associated with data, and incorporate these into your data lake design and implementation. Review your data strategy with
