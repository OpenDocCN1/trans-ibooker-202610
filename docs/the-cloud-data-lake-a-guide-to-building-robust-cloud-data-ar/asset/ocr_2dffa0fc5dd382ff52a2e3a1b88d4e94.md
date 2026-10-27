Data in the cloud data lake architecture can serve many purposes. However, there are two major scenarios that are common consumption patterns in an organization:

# *Business intelligence*

Data is used by BI analysts to create dashboards or work on interactive queries to answer key business problems that are well defined and work on highly structured data.

# *Data science and machine learning*

Data is used by data scientists and machine learning engineers to do exploratory analysis and experimental work to answer complex problems that don’t have a defined ruleset and require multiple iterations to get better. The data involved here assumes no structure.

# Modern Data Warehouse Architecture

In a modern data warehouse architecture, both the data lake and the data warehouse peacefully coexist, each serving a distinct purpose. The data lake serves as low-cost storage for a large amount of data and supports exploratory scenarios such as data science and machine learning. The data warehouse stores high-value data and powers dashboards used by the business. It is also used by BI users to query the highly structured data to gain insights about the business.

# Reference Architecture

Data is first ingested into a data lake from various sources: on-premises databases, social media feeds, and so on. This data is then transformed using big data analytics frameworks like Hadoop and Spark, where multiple datasets can be aggregated and filtered to generate high-value structured data. This data is then loaded into a cloud data warehouse to power dashboards, including interactive dashboards for BI analysts using their very familiar tool of choice—SQL. In addition, the data lake empowers a whole new set of scenarios that involve exploratory analysis by data scientists as well as machine learning models that can be fed back into their applications. A simplified representation of the modern data warehouse architecture is shown in Figure 2-8.