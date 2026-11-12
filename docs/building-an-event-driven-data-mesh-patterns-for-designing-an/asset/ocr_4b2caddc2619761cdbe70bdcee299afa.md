However, the best way to determine which domain data is necessary for others to use is to simply ask them. Take a customer-focused approach and lean into the needs of those business processes that depend on your domain. And this brings us to the next data mesh principle.

# Principle 2: Data as a Product

This principle elevates data to the status of first-class citizen, the same as any other product created by the organization. This is a complete reenvisioning of how data is created, stored, and communicated to other domains. By applying the same product rigor of existing production products, we can formalize how data is built and communicated across domains.

The domain owners are responsible for identifying, extracting, and modeling their data to present as a data product for external access. After all, who better than the domain model experts to construct the data for export? And like any other product, prospective consumers are identified and consulted to ensure that the data product is built to serve their needs. But a data product isn’t just the data itself—it’s also the code that builds it, the infrastructure that stores it, and the ports (or modes) by which you can access it, as shown in Figure 2-2.

![](bbox=[0.145, 0.455, 0.855, 0.66])

Figure 2-2. The code, data, infrastructure, and ports that comprise a data product

These four components are common to all data products. Every data product creator is going to need to identify and construct their data products with these in mind.

We’ll streamline the creation and management of data products, much like we do with microservices, to reduce the overhead and toil of each data product owner. We want it to be easy to build and support data products without getting bogged down in the operational overhead. We’ll cover self-service later in this chapter, but keep it in mind as we talk about serving data as a product.