![](bbox=[0.236, 0.085, 0.757, 0.405])

Figure 3-15. Data products are segregated via containers using a central shared storage account instance that is provisioned within the data management landing zone

This scenario of using a data management landing zone for data product management might be preferable when moving data closer to centrally managed services, such as MDM or data quality services. In these cases, it sometimes might be better to have data closer to where it is processed.

> You do not need to make an explicit choice for how you would like to manage your data products for all domains. You can combine any or all of the previously discussed approaches and blend them within the single landing zone topology or any other topology discussed in this chapter. For example, you could provide dedicated storage accounts for the majority of your domains, while sharing resources for some other domains. In the end, it's all about making the right trade-offs.

# Scaling a single landing zone

Let's leave data product management for now and go back to the data landing zone. The rationale for using a single data landing zone is to apply standardization and keep things simple. It's appropriate in cases where you use only one cloud provider and provision all resources in a single region, you don't have complex billing or account structure requirements, and you want or need to baseline all services and policies across all domains. When applying this form of simplification and standardization, there should be no (or few) contrasting requirements between domain teams.