| Data source | Benefits | Weaknesses |
|---|---|---|
| Data lake | • If using a data lakehouse architecture, you will find many of the same benefits as a data warehouse.
• Data is generally stored in its native format, which can be good for predictive analytics.
• Data may be continuously updated. | • If not using a lakehouse architecture, you may find performance for business intelligence poor because so much transformation is required to conform and shape data.
• Data lakes are generally shared with data scientists, data engineers, and analysts. Security can be a concern.
• The use of raw data increases the risk of users seeing different variations of the same data because the data has not been validated to the same extent as in a data warehouse or mart. |

In general, we recommend starting with a data mart or data warehouse connection if this already exists in your organization’s architecture. You’ll find it gives the best balance of accessibility, performance, and governance for your needs. A data lake or lakehouse, if well managed, offers some similar advantages. However, if you don’t already have a formal data warehouse or lakehouse in place, it is a significant effort to build one and likely beyond the scope of your embedded analytics project on its own.