# See also

+   - Use Performance analyzer to examine report element performance in Power BI Desktop: `https://bit.ly/3eWXdEb`

# Pushing Query Processing Back to Source Systems

During the scheduled refresh of datasets retrieving from on-premises sources, any query transformations not executed by the source system will require local resources of the **M (Mashup)** engine of the on-premises data gateway server. With larger datasets, and potentially with other scheduled refreshes occurring on the same gateway server at the same time, it becomes important to design M queries that take full advantage of source system resources via query folding. Although transformations against some sources such as files will always require local resources, in many scenarios M queries can be modified to help the engine generate an equivalent SQL statement and thus minimize local resource consumption.

In this recipe, a process and list of items is provided to identify queries not currently folding and the potential causes. Additionally, a query based on an existing SQL statement is redesigned with M expressions to allow query folding.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. Identify the dataset to evaluate for query folding
+   2. Use gateway server memory performance counter data to establish a baseline of the resources currently used to perform refreshes as these counters should be impacted by any changes

Also, establish a baseline for query performance using the new **Diagnostic tools** within Power Query Editor's **Tools** tab.

The dataset to be optimized will generally be a large PBIX file (100 MB+) published to the Power BI service with a scheduled refresh configured to use an on-premises data gateway and that queries a relational database (SQL, Oracle) as the primary source. If the large PBIX file is retrieving from a file or a collection of files within a folder, revisions are certainly possible, such as filtering out files based on their modified date relative to the current date, however, query folding is not an option for file sources.