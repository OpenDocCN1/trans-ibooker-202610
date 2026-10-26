
## Chapter 6. Improving Visibility

Even with transparent communication, you’re going to encounter roadblocks and issues both during the development and the operational phases of every initiative. Issues can stem from inadequate processes, bugs, edge cases, or unforeseen user behavior. As the group responsible for managing applications and systems in production, the question that your group should ask is, “How can we minimize the impact of a mistake or an unforeseen event?”

In a previous chapter, I’ve discussed the value of reducing Mean Time To Recovery (MTTR). The first step in that process would be identifying that there is an issue that needs to be resolved. The second is to quickly understand relevant information about the event. That’s where observability comes in. It provides an organization with an ability to inspect the state of the systems at every step and detect changes, allowing you to assess and provide feedback on suboptimal processes and changes in behavior. Monitoring should be treated as a continuous feedback loop for determining the performance of each metric at any given time.

However, because a system’s purpose is to support business objectives, focusing solely on the system’s health is not sufficient to guarantee the business’ performance. While major issues such as outages that substantially disrupt user experience have a clear impact on business performance, more subtle issues can have an even larger impact on business operations despite the appearance of technical success. For example, an error in business logic that does not properly act on a failure condition can prevent a small percentage of web application users from completing a purchase for an extended period of time. Although a change in revenue trends might eventually be noticed by the finance group, system metrics such as uptime and network latency would appear within normal levels, allowing the issue to persist, potentially leading to a more substantial loss of revenue than would occur from a widespread but short outage.

Data without context can lead to an inaccurate understanding of what’s actually affecting your systems and your business. This chapter covers both the importance of continuous insight into business performance—including what kinds of monitoring and alerting metrics you should consider—and the importance of data correlation.

### The Importance of Holistic Monitoring

Monitoring has traditionally been a function of IT, focusing on *system health*. However, the health of the system is not valuable as a standalone measure. For organizations that rely on technology for their business success, monitoring efforts that are isolated to technical symptoms are at high risk for continuous failures. Focusing on metrics related to business success is vital for teams responsible for the health of the supporting system.

> *I don’t give a **** if my whole datacenter is on fire, as long as I am still making money.*
> 
> CEO

The operations group needs to have access to business metrics of success in order to have a holistic view of the *business’s health* and to be able to properly detect, diagnose, remedy, and (potentially) predict business-disrupting issues:

appropriate team would be notified immediately. An extensive rule set for “everything is working” criteria has been established, including HTTP checks returning a 200 code from target API checks, system health being within norm, and the user browsing trends being within defined thresholds. The assumption was that while those metrics were within the norm, from a system operations perspective, the site was operational. That criteria was serviceable until one of the issues the team encountered was silent loss of event data.

The site was up and available for browsing and the API accepted post requests and returned success codes to users, but the posts themselves never registered in the database due to a faulty code deployment. From the standpoint of the group responsible for operational stability, the available data showed that the site was up and running; frustrated users and business analysts had a different experience.

The issue, in this particular case, was that the team responsible for service availability had concentrated on monitoring system and application metrics. Even though these metrics are important for isolating and troubleshooting the problem, the first symptom of the problem should be a change in a business metric trend. Setting the system to collect data on average events per second and setting an appropriate threshold alert on it would immediately raise flags after deployment, allowing for much faster Mean Time To Detection (MTTD).

### A Top-Down Monitoring Strategy

There are a number of reasons an effective monitoring strategy is beneficial. For one, because software can never be bug-free, constantly observing system behavior is the only way to detect production problems early. The complexity of modern systems is another reason, where a glitch in one part of the architecture can have a trickle-down effect throughout the entire system, manifesting as a completely different cause. And, of course, effective monitoring allows you to proactively detect potential problems such as user-detected failures, security breaches, and performance spikes.

But perhaps the most important reason to monitor and trend the behavior of your system is because things change. And changes affect business.

A change anywhere in the organization can have a trickle-down effect involving processes from completely different groups. As a person whose responsibility it is to maintain the health of the underlying systems that support business, you need to be able to detect the changes as well as understand the complete scope of their effects. Follow the steps laid out in the following subsections when defining your monitoring strategy.

### Understand the Business Objectives

As discussed in Chapter 3, the first step toward establishing an effective monitoring program is to understand your organization’s business objectives. Clearly identifying these objectives helps to come up with a list of business-critical metrics that need to be continuously observed for change. Understanding the success baseline values of those metrics enables the changes in data patterns to be evaluated based on the value of change.

For example, an organization that relies on site registration (beta sign-ups, membership services) might want to begin with a business check to make sure the hourly number of registrations does not drop below a set threshold. Similarly, an ecommerce application could benefit from a business metric that monitors credit card transaction success-versus-failure ratio to ensure that the sales process works as expected. Metrics such as these not only provide considerable business intelligence for forward decision making, they also provide a framework of focus for technical metrics in support of these benchmarks.

### Map Business and Technical Metrics

After you’ve mapped the business objectives, the next step is to identify the technical metrics that affect the associated business metrics. Mapping the relationships between technical metrics and business objectives provides context and highlights mutual impacts. For instance, the credit card transaction success-versus- failure ratio measured by an ecommerce business depends on the availability of the application server, connectivity to a third-party authorization service, and availability and performance of that service itself. If an outage of the authorization service occurs, it will translate into a higher ratio of card transaction failures. Conversely, a higher ratio of card transaction failures might be caused by an issue highlighted by one of the associated technical metrics.

### CORRELATE DATA

Continuous monitoring can (and should) collect large volumes of data for analysis. After the metric sets for each level have been established, make sure that the related metrics can be easily correlated. Collecting multiple business and technical metrics helps to ensure that the necessary information is *available*, but having the ability to review the related metrics together helps to ensure that the necessary information is *useful*. Understanding the relationships accelerates troubleshooting by enabling issues identified through a business metric to be mapped to the related technical metrics (and vice versa). Don’t underestimate the importance of cross-silo correlation. More often than not, interpreting the reason behind a change in one metric requires following a chain of impact through multiple metrics, often collected via different channels. Having that ability to easily traverse the dependency tree on demand would improve MTTR.

Let’s examine these premises by the way of a practical example. Most incident responses start with a call (or a page), and this one was no different. In this case, a call came from the CEO of a large online marketing company, operating a very complex system with multiple revenue-generation points supporting more than 100 million users. As he was reviewing his business health dashboard, the CEO noticed that the revenue from online channels had dropped by more than 20% from the normal daily threshold. He immediately escalated the issue to his web operations team.

As any engineer knows, step one of troubleshooting the problem is to confirm the problem, so looking at the revenue trends seemed like a good starting point. Fortunately, the WebOps team had access to business KPI metrics, so it was able to pull up the information in its monitoring system, as shown in Figure 6-1.
