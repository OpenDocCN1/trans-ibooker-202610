when combining data. Therefore, I recommend managing the consuming side jointly with your data product and master data management activities.

# DDSs Versus Data Products

The goal of data products is providing data. DDSs have the opposite goal: consuming data and turning it into value. They’re about making business decisions, supported by functions such as reporting and machine learning services. The data that is consumed by DDSs also differs from the data that is typically managed by data product architectures. Data in DDSs has been structured and optimized for the use case at hand. Each business problem is unique and may involve different target user groups, different business requirements, and different nonfunctional requirements.

DDSs may have some overlap with data products when it comes to distributing newly created data, however. We’ve already discussed that DDSs can play the role of data consumer, provider, or sometimes both. That is, DDSs may share newly created insights and data with other applications. To facilitate this, you need to carefully consider how to align the data providing and consuming sides of your architecture. As you can see in Figure 11-4, there are two design patterns you can choose from:

+   - Domain A uses one architecture to solely consume and use data and another for data product development.
+   - Domain B uses a combined architecture that supports both data value creation and data product development.

![](bbox=[0.141, 0.535, 0.856, 0.863])

Figure 11-4. DDSs and data products can be managed together or independently