
- 5. Launch the DAX Studio standalone Windows application.
- 6. Connect to a Power BI Desktop file or SSAS tabular instance.

The **Advanced Options** settings of the **Connect** dialog box allow you to control exactly how DAX Studio connects to the model, such as the ability to specify a security role, effective user name identity, and **locale** when defining connections to data models and when analyzing trace events associated with DirectQuery data models (that is, the SQL statements generated and passed to sources).

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_41_0.png)

Figure 1.28: DAX Studio advanced connection options

### There's more...

There are two additional tools that are useful for Power BI. Namely:

- ALM Toolkit
- Tabular Editor

The ALM Toolkit is a third-party tool from MAQ Software that provides advanced features such as Power BI dataset comparison, code merging, partial deployments and bug fixes, source control integration for dataset metadata, and definition reuse between tabular models. To download and install the ALM Toolkit, follow these steps:

- 1. In a browser, navigate to http://alm-toolkit.com.
- 2. Near the top of the page, click the **DOWNLOAD LATEST VERSION** button.
- 3. Use the **Open file** link to launch AlmToolkitSetup.msi or open the file from your **Downloads** folder.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_42_0.png)

Figure 1.29: ALM Toolkit installer

- 4. Click the **Next** button on the initial installation screen.
- 5. Accept the license agreement and click the **Next** button.
- 6. Choose a **Folder** path and click the **Next** button.
- 7. On the final installation screen, click the **Next** button to start the installation.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_42_1.png)

Figure 1.30: Successfully installed ALM Toolkit

The Power BI ALM Toolkit can now be launched from the Windows Start menu.

Another powerful tool is Tabular Editor. At the time of this book being published, Tabular Editor comes in two versions, the free, open source version 2 and the commercial version 3. Tabular Editor is an alternative to **SQL Server Data Tools (SSDT)** for authoring and editing tabular models for Analysis Services. Tabular Editor provides a hierarchical view of the objects in your tabular model metadata, such as columns, measures, and hierarchies. Tabular Editor integrates with Power BI Desktop, allowing batch changes to DAX measures and enabling advanced capabilities such as calculation groups and perspectives. Finally, Tabular Editor also enables offline editing capabilities by allowing you to open the tabular model directly from Model.bim files. To install and use Tabular Editor, follow these steps:

- 1. Download the latest Tabular Editor from here: https://bit.ly/3bJFBv1.
- 2. Scroll to the bottom of the page and click the link for **TabularEditor.Installer.msi**.
- 3. Once the file downloads, use the Open file link or open the file from your Downloads folder.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_43_0.png)

Figure 1.31: Tabular Editor installer

- 4. Click the Next button on the initial installation screen.
- 5. Accept the license agreement and click the Next button.
- 6. Choose a Folder path and click the Next button.
- 7. Check the boxes Create Desktop shortcut and Create Program Menu shortcut, and then click the Next button.
- 8. On the final installation screen, click the Next button to start the installation.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_43_1.png)

Figure 1.32: Successfully installed Tabular Editor

Tabular Editor can now be launched from the Windows Start menu.

### See also

- DAX Studio tutorials and documentation: https://daxstudio.org
- ALM Toolkit documentation: http://alm-toolkit.com/HowToUse
- Tabular Editor documentation: https://docs.tabulareditor.com/

## Conclusion

In this chapter, we walked through the installation and configuration of the primary tools that BI professionals utilize to design and develop Power BI content, including official Microsoft tools such as Power BI Desktop, the On-premises data gateway, and Analyze in Excel, as well as third-party tools such as DAX Studio, the ALM Toolkit, and Tabular Editor. These tools, coupled with the Power BI service, are the primary resources needed by BI professionals to be productive with Power BI. Later chapters of this book explore the use of these tools in developing and enhancing Power BI solutions.

## Accessing and Retrieving Data

- Applying Multiple Filters
- Selecting and Renaming Columns
- Transforming and Cleansing Source Data
- Creating Custom Columns
- Combining and Merging Queries
- Selecting Column Data Types
- Visualizing the M Library
- Profile Source Data
- Diagnosing Queries

### Technical Requirements

The following are required to complete the recipes in this chapter:

- Power BI Desktop
- SQL Server 2019 or newer with the **AdventureWorksDW2019** database installed. This database and instructions for installing it are available here: http://bit.ly/20VQfG7

### Viewing and Analyzing M Functions

Every time you click on a button to connect to any of Power BI Desktop's supported data sources or apply any transformation to a data source object, such as changing a column's data type, one or multiple M expressions are created reflecting your choices. These M expressions are automatically written to dedicated M documents and, if saved, are stored within the Power BI Desktop file as Queries. M is a functional programming language like F#, and it is important that Power BI developers become familiar with analyzing, understanding, and later, writing and enhancing the M code that supports their queries.

### Getting ready

To prepare for this recipe, we will first build a query through the user interface that connects to the **AdventureWorksDW2019** SQL Server database, retrieves the **DimGeography** table, and then filters this table to a single country, such as the United States:

- 1. Open Power BI Desktop and click on **Get Data** from the **Home** tab of the ribbon. Select **SQL Server** from the list of database sources. For future reference, if the data source is not listed in **Common data sources**, more data sources are available by clicking **More...** at the bottom of the list.
- 2. A dialog window is displayed asking for connectivity information. Ensure that **Data Connectivity mode** is set to **Import**. Enter the name of your SQL Server as well as the **AdventureWorksDW2019** database. In *Figure 2.1*, my SQL Server is installed locally and running under the instance **MSSQLSERVERDEV**. Thus, I set the server to be **localhost\MSSQLSERVERDEV** to specify both the server (localhost) and the instance. If you leave the **Database** field blank, this will simply result in an extra navigation step to select the desired database.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_47_0.png)

Figure 2.1: SQL Server Get Data dialog

- 3. If this is the first time connecting to this database from Power BI, you may be prompted for some credentials. In addition, you may also be warned that an encrypted connection cannot be made to the server. Simply enter the correct credentials for connecting and click the **Connect** button. For the encryption warning, simply click the **OK** button to continue.
- 4. A navigation window will appear, with the different objects and schemas of the database. Select the **DimGeography** table from the **Navigator** window and click the **Transform Data** button.
- 5. The Power Query Editor launches in a new window with a query called **DimGeography**; preview data from that table is displayed in the center of the window. In the Power Query Editor window, use the scroll bar at the bottom of the central display area to find the column called **EnglishCountryRegionName**. You can also select a column and then click **Go to Column** in the ribbon of the **View** menu to search for and navigate to a column quickly. Click the small button in the column header next to this column to display a sorting and filtering drop-down menu.
