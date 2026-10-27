
## See also

- Incremental refresh in Power BI: `https://bit.ly/3f3DSkM`
- Incremental refresh for datasets in Power BI: `https://bit.ly/3w10oSv`

## Conclusion

This chapter's recipes contained top data modeling, DAX measure, and M query patterns to enhance the performance, scalability, and reliability of Power BI datasets. This included performance tuning examples of both data models and measures, error handling, and query folding examples of M queries in addition to supporting details on the DAX and M query engines. The next chapter is all about deploying and distributing Power BI content using the Power BI service, Teams, and mobile devices.

## 12

## Deploying and Distributing Power BI Content

Thus far, much of this book has focused on the individual BI professional working with Power BI to create data models, reports, dashboards, and other BI and system assets. However, BI is all about providing insights and information to the business. Thus, for any BI project to be successful, the data and insights created must be shared and distributed throughout the organization.

This chapter contains detailed examples and considerations for deploying and distributing Power BI content via the Power BI service and Power BI mobile application. This includes the creation and configuration of app workspaces and apps, procuring and assigning Power BI premium capacities, configuring data sources and refresh schedules, and deriving greater value from the Power BI mobile application. Additionally, topics such as staging deployments across development and production environments, as well as multi-node premium capacity deployments, are covered.

In this chapter, we will cover the following recipes:

- Preparing for Content Creation and Collaboration
- Managing Content between Environments
- Sharing Content with Colleagues
- Configuring Workspaces
- Configuring On-Premises Gateway Data Connections
- Publishing Apps

## Deploying and Distributing Power BI Content

- Publishing Reports to the Public Internet
- Enabling the Mobile Experience
- Distributing Content with Teams

At the end of this chapter, you will be well informed about how to deploy and distribute your Power BI content.

### Technical Requirements

The following are required to complete the recipes in this chapter:

- Power BI Desktop.
- SQL Server with the AdventureWorksDW2019 database installed. This database and instructions for installing it are available here: http://bit.ly/20VQfG7.
- Files for this chapter and can be downloaded from the following GitHub repository: https://github.com/PacktPublishing/Microsoft-Power-BI-Cookbook-Second-Edition

### Preparing for Content Creation and Collaboration

Power BI collaboration environments can take many forms, ranging from a small group of Power BI Pro users creating and sharing content with each other in a single app workspace to large-scale corporate BI scenarios characterized by many read-only users accessing Power BI premium capacity resources via Power BI apps. Given the cost advantages of the capacity-based pricing model Power BI Premium provides, as well as the enhanced performance and scalability features it delivers, it is important to properly provision and manage these resources.

This recipe provides two processes fundamental to the overall purpose of this chapter: deploying and distributing Power BI content. The first process highlights several critical questions and issues in planning and managing a Power BI deployment. The second process details the provisioning of Power BI Premium dedicated capacity resources and the allocation of those resources to specific deployment workloads via app workspaces.

### Getting ready

There are five main licensing mechanisms for Power BI

- Power BI Free
- Power BI Pro
- Dedicated capacity (Embedded, Premium)
- Premium Per User
- On-premises (Power BI Report Server)

Power BI Free provides extremely limited sharing and distribution features (Publish to web) while Power BI Pro and Premium Per User require every user that creates or consumes Power BI content to be licensed. Dedicated capacities such as Embedded and Premium allow free users to consume content published to embedded or premium dedicated capacity workspaces. On-premises implementations are licensed via Power BI Report Server, a superset of SQL Server Reporting Services (SSRS).

It is essential to carefully review the licensing and features associated with each deployment option. For example, many of the features in the Power BI service such as dashboards and Q&A (natural language queries) are not available in on-premises Power BI Report Server. Likewise, certain Power BI Premium SKUs are exclusive to embedding Power BI into applications. For a more complete treatment of Power BI licensing options, see the See also section of this recipe.

### How to do it...

To implement this recipe, do the following:

1. Determine how Power BI content (datasets, reports, and dashboards) will be deployed and consumed by users.
   - Will content be deployed to the Power BI service and accessed via apps and Power BI mobile apps?
   - Will content be deployed to the Power BI service but embedded into business applications?
   - Will content be deployed to the Power BI report server on-premises and accessed via the reporting services web portal as well as the Power BI mobile app?

For hybrid deployments, such as using both the Power BI service and embedding, or using the Power BI service and Power BI Report Server, estimate the resources required for each of these workloads and evaluate both a consolidated licensing model and separate, dedicated licenses. For example, if 16 virtual cores are provisioned with a Power BI premium P2 SKU, 16 separate cores are also available for licensing Power BI Report Server on-premises. Such consolidated licensing may be more advantageous than licensing Power BI Report Server separately and paying for additional Power BI Pro licenses.

## Deploying and Distributing Power BI Content

2. Identify or estimate the Power BI Pro and Power BI Free users based on their roles and needs in the organization.
   
   - Will the user create and publish content (Power BI Pro)?
   - Will the user only consume content and optionally create content for their personal use (Power BI Free)?

Connecting to published datasets via Analyze in Excel and Power BI service Live Connections are Power BI Pro features and are thus not available to Power BI Free users even if the dataset is assigned to a Power BI Premium capacity. However, a Power BI Free user can still view and subscribe to reports and dashboards in Premium capacities, and can also export content to CSVs and PowerPoint.

3. For larger deployments with many read-only users, estimate the Power BI Premium resources required.
   
   - Use the Power BI Dedicated Capacity Load Assessment Tool referenced in the See also section as a starting point.
   - Plan how deployment workloads will be allocated across premium capacity nodes.
   - Will a given workload (or perhaps a business function) have its own capacity, or will a single, larger capacity support multiple or all workloads or teams?

If Power BI datasets in import mode will serve as the primary data storage option supporting reports and dashboards, consider their memory usage relative to the memory available per Power BI Premium SKU. For example, 25 GB of RAM is currently available in a P1 capacity node, and this may be insufficient for larger dataset (model) sizes stored in the service with a scheduled refresh. Like SSAS tabular models, 2.5X of memory should be provisioned to support both processing and refreshing, queries, and temporary memory structures created during queries.

4. Evaluate and plan data modeling tools (datasets).
   
   - Will Power BI Desktop be used for both data modeling and report development or will Analysis Services be used for data modeling and Power BI Desktop only used for reporting?
   - What will the storage mode of these new semantic models be (Import, DirectQuery, Composite) and what are the implications for required memory? For example, aggregations are exclusively available with Power BI Premium and thus significantly less memory would need to be provisioned if aggregations are used correctly.
   - Are changes to a relational data source or infrastructure necessary to support performance?

In some scenarios, the relational data source must be revised or enhanced to support sufficient DirectQuery performance. These enhancements vary based on the source but may include indexes (such as Columnstore indexes in SQL Server), greater compute and memory resources, denormalization, and referential integrity.
