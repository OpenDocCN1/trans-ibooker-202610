removes it or fills it with legitimate values (e.g., looks up the zip code from the address).

# *Optimizations*

Once the data adheres to a tabular structure, it is also important to prepare the data to be optimized for the most common consumption patterns. The most common pattern is for data to be written once and read many times. For example, if you have a sales database, the most common queries on this data tend to be for region-specific sales information or trends over time. In this step, data is converted into a format and reorganized to be friendly for the most common queries. Typically, a columnar format like Parquet, which we saw in “Data formats” on page 45, is selected to be optimized for queries.

This data is used more broadly by the different teams, data scientists, and, in some cases, business analysts as well for more ad hoc analysis. Data in this zone is organized into domains, or units that the various consumers can understand, and is published in a catalog (we will talk more about this in “Introduction to Data Governance” on page 78).

# *Curated data (Gold) zone*

The curated data zone has the highest-value data in the data lake and critical datasets that power key business dashboards. Think of the data in this zone as a digest or a summary of the value in the data, the key insights. Data in this zone is processed by performing aggregations, filtering, correlating data from different datasets, and other complex calculations that provide the key insights into solutions for business problems. The data in this zone uses data in the curated zone as its source, along with other data sources occasionally. Data in this zone needs to adhere to the highest standards in terms of data quality, given its broad impact to the business.

Curated data is mostly used by business analysts and powers dashboards that are leveraged by key business decision makers. Additionally, this data is published in a catalog and is organized by the business units and the domains it is relevant to.

# *Workspace or sandbox zone (optional)*

As we discussed, the data that is present in the raw, enriched, and curated zones is largely managed by a select set of data engineers or data platform teams. However, there are datasets that consumers would like to bring, either for exploratory analysis or to test the waters. This data can be organized into units that are specifically provisioned for the user (think of this like a scratch pad). Data here adheres to no specific standard or quality and is essentially available for any free-form use.

Figure 3-5 illustrates the data organization at Klodars Corporation.