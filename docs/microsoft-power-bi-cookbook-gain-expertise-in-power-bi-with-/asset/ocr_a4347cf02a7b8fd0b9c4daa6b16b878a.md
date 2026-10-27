# Getting ready

A stakeholder matrix can help structure the report design planning process around the needs of the different parties accessing the model and reports. The stakeholders or business units, such as Merchandising, appear on the columns axis and replace the conformed dimensions (that is, Product or Vendor) that were used in the data warehouse bus matrix described in Chapter 3, Building a Power BI Data Model.

|  | Stakeholders |  |  |  |  |  |  |  |
|---|---|---|---|---|---|---|---|---|
| BUSINESS PROCESSES | Executive | Sales | Finance | Marketing | Merchandising | eCommerce | Customer Service | Supply Chain |
| Internet Sales | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |  |  |
| Internet Sales Plan | ✓ | ✓ |  | ✓ |  | ✓ | ✓ | ✓ |
| Reseller Sales | ✓ | ✓ | ✓ |  | ✓ |  |  |  |
| General Ledger | ✓ |  | ✓ |  |  |  |  |  |
| Inventory |  |  | ✓ | ✓ | ✓ | ✓ |  |  |
| Customer Surveys | ✓ | ✓ | ✓ | ✓ |  | ✓ | ✓ | ✓ |
| Customer Service Calls |  |  | ✓ |  |  |  | ✓ |  |
| Shipping |  |  | ✓ |  | ✓ |  |  | ✓ |

Figure 4.1: Stakeholder matrix

In this example, the data model contains the four highlighted fact tables: Internet Sales, Internet Sales Plan, Reseller Sales, and General Ledger. When there can be multiple stakeholders within a given business function with their own unique needs and use cases, these stakeholders can be added as columns to the stakeholder matrix. In Power BI, there are many options for meeting the unique needs of different stakeholders with the same data model and underlying retrieval and data architecture.

# How to Build Reports

While report development should be an agile process where feedback is regularly solicited from stakeholders as the report is designed and developed, having a solid planning and design process for a report upfront is extremely beneficial in understanding the scope and requirements of it. A single report should not attempt to address unrelated business questions or meet the needs of highly diverse users such as a corporate financial analyst or a store manager. Multi-scope reports can lead to convoluted user experiences and report-level customization that can be difficult to maintain and scale. The report planning and design process should answer the following five questions:

+   1. Who will be accessing this report?
+   2. What are the top priorities of this report in terms of business questions?
+   3. How will the report be accessed and utilized?