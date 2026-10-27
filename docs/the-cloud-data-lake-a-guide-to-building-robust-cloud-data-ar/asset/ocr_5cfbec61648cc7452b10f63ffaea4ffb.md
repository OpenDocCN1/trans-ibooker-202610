and Azure Data Lake Storage offers a truly no-siloed data store that supports multiple protocols, including Network File System (NFS) version 3.

# *Integration with your other cloud services*

If you already have an implementation with a particular cloud provider, then that is the best starting point for your cloud data lake implementation. You could extend your current cloud implementation into the data services. As an example, if you already have a web application running on the cloud, your data platform could leverage ingesting logs or clickstream data from the web application to enable scenarios like customer dashboards or personalization features to your web application. This is a good starting point because you’re starting on familiar terrain and extending an existing implementation.

The best way to assess the capabilities of the cloud providers is to do a PoC implementation of your scenarios. This will help you understand the best fit for your needs while also informing you about the cost profile of these solutions. Pick a PoC that best illustrates the needs of your customers. As an example, when Klodars Corporation is evaluating cloud data lakes, it can pick two of its highest-priority problems:

+   - The sales dashboards are taking a long time to refresh, disrupting the executive review meetings.
+   - The marketing team needs to understand how to best target its social media campaigns based on analysis of existing trends.

When Klodars did a PoC on the three cloud providers, it found that AWS Lake Formation offered the simplest implementation for its cloud data lake; Azure Data Lake Storage offered the lowest cost as well as interoperability with its on-premises systems with NFS v3 support; and Google BigQuery offered the fastest performance on both structured and unstructured data while scaling better than the data warehouse solutions on AWS and Azure. Given that Klodars’s biggest pain point is around performance, it picked GCP for its cloud data lake implementation.

# Decision points for data lake migrations

If you have an existing on-premises infrastructure (either data lake or data warehouse) that you would like to migrate to a cloud data lake architecture, then you have a few choices to make. These can be bucketed under three major design choices:

# *Lift and shift*

In this design choice, you are preserving your existing design and primarily changing the hosting to the cloud. The advantage of this approach is it requires minimal refactoring of your code and components, so you can get going on the cloud sooner. Most public cloud providers like AWS, Azure, and GCP offer pro-