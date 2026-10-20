and reporting. This is where the *star schema* comes in. The star schema is a widely used approach for modeling data. Ralph Kimball introduced it in 1996, and it has remained a best practice ever since, due to its simplicity, performance, and flexibility. While it’s not exclusive to Microsoft Fabric, the star schema remains highly relevant to building efficient data models on modern platforms.

In a star schema, data is divided into two main types of tables: fact tables and dimension tables. *Fact tables* store actual events or numeric values (such as sales amounts or quantities), while *dimension tables* describe the context of those values (such as products, customers, time, or locations).

This design offers several advantages. It helps users navigate the data more intuitively, and queries perform better because the structure minimizes the number of joins. Additionally, the model is easier to maintain and adapt when the business changes (for example, if a customer moves or a product is rebranded).

In summary, we choose a star schema because it lets us create fast, scalable, and easy-to-understand data models for a wide array of users. For instance, we can import a star schema developed within a Fabric lakehouse or warehouse into Power BI.

Due to the design of the star schema, Power BI developers can easily establish relationships, resulting in a data model that is both clear and reliable.

Figure 3-34 shows an example of a star schema.