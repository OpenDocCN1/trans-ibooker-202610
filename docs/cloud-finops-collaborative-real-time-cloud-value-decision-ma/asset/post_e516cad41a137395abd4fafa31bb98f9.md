
Instead of a focus on centralizing spend approvals, FinOps builds visibility of the spend to the appropriate areas to create accountability. The cloud-aware expertise of the FinOps team allows the other teams to understand how each specific billable item can be distributed into chargeback and showback. These are terms used for how you display and handle costs. We'll dig into this subject more in Chapter 11.

## Why a Centralized Team?

We have so far referred a lot to the FinOps team and to the centralized functions a FinOps team performs. It's worth pointing out here that we'll continue to refer throughout the book to a centralized FinOps team, but there are a number of different FinOps Team models that work. How your FinOps team is structured will largely depend on the type of organization you have and the way it will support a cross-discipline, coordination machine that the FinOps team represents.

While we will still refer to the FinOps team, interpret that as the coordinating body that drives FinOps best practices, but understand that whatever organizational design that takes, there are things that a FinOps team should do organization-wide to make the transformation as successful as possible.

The unbiased central team, with ties to business leaders, engineering teams, and finance teams, shares objective best practices and recommendations. The members of this team are never seen as pushing a specific agenda to benefit themselves, which builds more trust in the advice they give. Critically, they must also work to incorporate business objectives for each cloud workload.

When this central group drives the rules of cost allocation and pushes out clear communication about how teams are doing, everyone is reassured that they're being managed and measured against the same criteria. If one of the budget-holding teams drives the process, which happens when a group is responsible for the lion's share of cloud spend, it can raise questions about the integrity of information. If cost allocation is done independently by each team, there will inevitably be overlap or gaps in spending data. In either case, doubt grows. And when teams lose trust in data, they can't be held accountable to it.

When individual teams try to build out their own cloud reporting processes, disagreements arise about whose cost was whose. An organization-wide FinOps team solves this issue, fostering agreement that the spend data is the correct data and then objectively attributing each cost to the appropriate engineering group.

Finally, the central team defines what the organization uses as a business metric. In fact, this is a key early task of the FinOps team. A dollar is not just a dollar when it comes to cloud spending. One can choose to look at cost amortized or not amortized, and with custom rates applied or excluded. Shared costs of an application from another service may be included or invisible. When the business metric used across teams is clearly defined, everyone speaks the same language. In the next chapter, we'll dive deeper into the language of FinOps.

## The FinOps Team Doesn't Do FinOps

It’s easy to assume that the daily work of FinOps can be assigned to a central team and siloed from the work of the rest of the business. But FinOps isn’t only happening inside your FinOps team; they are primarily an enablement team that supports the rest of the organization with subject matter expertise in the very real complexities of cloud billing data and pricing models, centralized data and reporting, commitment management, and related functions while helping the distributed engineering teams with the data they need to make daily decisions about their infrastructure. It should not be expected to be a central team for managing infrastructure or performing the bulk of optimization activities. That responsibility is pushed to the groups that understand their own infrastructure and their business objectives the best. The FinOps team aims to propagate repeatable patterns throughout the organization to accelerate FinOps practices at the edges.

> > The other teams in our organization ask for me to tell them about FinOps and explain how we are going to protect them from overspending. To which I say, I'm not, I'm gonna help you protect yourself.
> >
> > —Alison McIntyre, Cloud FinOps at Lloyds Banking Group on The FinOpsPod, 2022

A FinOps team will do more of the work early on in an organization’s cloud maturity, as everyone becomes familiar with their new responsibilities. But as time goes on, teams will become more self-sufficient, and the FinOps team will be able to tackle fewer of the routine tasks and focus on more complex projects related to cloud efficiency.

> > Why is subject matter expertise in cloud billing needed? As an example, AWS’s S3 storage service has at least 6 different types of “actions” you can get charged for—storage pricing, request and data retrieval pricing, data transfer and transfer acceleration pricing, data management and analytics pricing, replication pricing, and the price to process your data with S3 Object Lambda. There are also at least 8 different storage types from Standard to Infrequent Access to Glacier to Deep Archive. That is 48 combinations of pricing options for a single service. Amazon currently has hundreds of different services all with different pricing options. Subject matter expertise in a FinOps team can help navigate this complexity.
> >
> > —Ashley Hromatko, formerly Director of Cloud FinOps at Pearson

FinOps teams also might, as they mature, pick up more responsibility for automating the really routine tasks that aren’t a result of engineering teams being inefficient. There is a certain amount of technical debt introduced when moving to cloud, such as putting storage in a more expensive tier than it needs to be or purchasing higher tiers of service than required, that the FinOps team might be empowered to fix on its own, once the FinOps team has established the credibility to do so and the trust with engineering teams.

This enablement pattern is not a new concept; consider security in the cloud. It's a shared responsibility model. Just as the security team in an organization is not solely responsible for security, the FinOps team is not there to optimize all the things. It's there to provide the resources for the engineers closest to the cloud resources to decide on the right optimizations at the right time, or to not optimize them using *informed ignoring*, covered in Chapter 2.

The work that your central FinOps team does enables and accelerates your organization to use cloud more effectively. It doesn't replace the daily duties that other engineering teams and business leaders have to continuously optimize their own infrastructure.

## The Role of Each Team in FinOps

Figure 3-1 demonstrates how organizations operate in the FinOps model. A cross-functional team, such as the Cloud Center of Excellence (CCoE), manages the cloud strategy, governance, and best practices and then works with the rest of the business to transform how the cloud is used.

![](img/4d4736bc293fd892d1da81802d0f9520_64_0.png)

Figure 3-1. Team interaction around FinOps

Individuals at every level and in every area of an organization have different roles to play in the FinOps practice. These include the following.

### Executives and Leadership

Executives (e.g., VP/Head of Infrastructure, Head of Cloud Center of Excellence, CTO, or CIO) focus on driving accountability and building transparency, ensuring teams are being efficient and not exceeding budgets. They're also drivers of the cultural shift that helps engineers begin considering cost as an efficiency metric, discussed in Chapter 24.

### Engineering and Developers

Engineers and ops team members—such as Lead Software Engineer, Principal Systems Engineer, Cloud Architect, Service Delivery Manager, Engineering Manager, or Director of Platform Engineering—focus on building and supporting services for the organization. Cost is introduced as a metric in the same way other performance metrics are tracked and monitored. Teams consider the efficient design and use of resources via activities such as rightsizing (the process of resizing cloud resources to better match the workload requirements), allocating container costs, finding unused storage and compute, and identifying whether spending anomalies are expected.

### Finance

Finance team members use the reporting provided by the FinOps team for accounting and forecasting. They work closely with FinOps practitioners to understand historic billing data so that they can build out more accurate cost models. They use their forecasts and expertise from the FinOps team to engage in rate negotiations with cloud service providers.

### Procurement and Sourcing

Procurement and sourcing people manage the business’s relationship with vendors, including the cloud service providers. This relationship may be very different from other IT services relationships managed by the company in the past, and it may be more complex and varied to manage. They are interested in getting the best pricing for the commitments the company is making to each vendor, and making sure that the organization receives all the benefits of its patronage and commitments over time.

### Product or Business Teams

Product teams members, including Product Managers, Product Owners, Portfolio Owners, Service Owners, Application Leads, and the like are responsible for a service or product or product line. They will work closely with the FinOps team to understand, often in ways they could not when supported in the data centers, the total cost of running the features of their product or application. Product leads are interested in new features to solve customer needs, and will be able to use FinOps-provided data to understand product profitability, direct cost efficiency efforts, and more accurately forecast costs based on new feature releases.
