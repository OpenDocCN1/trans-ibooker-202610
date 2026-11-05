Let's take a minute to look at a diagram of the Northwind database. You can use the following diagram to refer to as we write code and queries throughout this book:

![](bbox=[0.07, 0.13, 0.92, 0.6])

Figure 10.1: The Northwind database tables and relationships

You will write code to work with the Categories and Products tables later in this chapter and other tables in later chapters. But before we do, note that:

+   - Each category has a unique identifier, name, description, and picture.
+   - Each product has a unique identifier, name, unit price, units in stock, and other fields.
+   - Each product is associated with a category by storing the category's unique identifier.
+   - The relationship between Categories and Products is one-to-many, meaning each category can have zero or more products.

# Using Microsoft SQL Server for Windows

Microsoft offers various editions of its popular and capable SQL Server product for Windows, Linux, and Docker containers. We will use a free version that can run standalone, known as SQL Server Developer Edition. You can also use the Express edition or the free SQL Server LocalDB edition that can be installed with Visual Studio for Windows.