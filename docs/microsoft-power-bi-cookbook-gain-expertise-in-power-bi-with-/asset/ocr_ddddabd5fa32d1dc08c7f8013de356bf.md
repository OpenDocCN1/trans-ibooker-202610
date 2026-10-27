# See also

+   - Four Different Ways to Find Your Power BI Local Port Number: https://bit.ly/3uiV01d
+   - Dynamic Management Views (DMVs) in Analysis Services: https://bit.ly/33etDoc

# Analyzing SSAS tabular databases and gateways

A Microsoft on-premises data gateway enables specific cloud services, including Power BI, Azure Analysis Services, Power Apps, and Power Automate, to securely connect to on-premises data sources. In the context of Power BI, these connections support both the scheduled refresh of imported datasets stored in Power BI, as well as DirectQuery and Live Connection datasets, in which only report queries and their results are exchanged between Power BI and the on-premises source. As the availability and performance of the gateway are critical for any Power BI and other supported cloud service deployment requiring on-premises data, regular monitoring of both the gateway service and its host server(s) is recommended.

In this recipe, performance monitor counters specific to the on-premises data gateway and SQL Server Analysis Services are integrated into a single Power BI dataset. This source data is dynamically retrieved and enhanced via M queries, and sample report visualizations are created to support monitoring and analysis.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. For the initial deployment or planning phases, review the available documentation, tips, and best practices on both SSAS tabular and the on-premises data gateway, including the recommended hardware and network configuration.
+   
+      SSAS tabular servers should have 2.5x the RAM of their compressed in-memory databases, and outbound port 443 should be opened to run the on-premises data gateway in the default HTTPS mode (ports 9350-9353 if TCP mode). Despite sufficient hardware, the design and complexity of data models, M queries, and DAX measures can significantly impact resource usage and performance. See *Chapter 11, Enhancing and Optimizing Existing Power BI Solutions*, for more details.
+   
+   2. Identify a secure network location directory to store the performance counter file. This path could use a common network drive and the parent folder of other monitoring log files.