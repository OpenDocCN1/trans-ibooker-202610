
## 产品需求文档（PRD）

**项目名称**：智能文档解析系统

**版本**：v1.2

**更新日期**：2025-06-15

### 1. 项目背景

随着企业数字化转型的深入推进，文档数量呈现指数级增长。传统的文档管理方式存在检索困难、信息利用率低、人工标注成本高等问题。本项目旨在构建一套智能文档解析系统，利用自然语言处理和光学字符识别技术，实现文档的自动分类、信息抽取和结构化存储。

### 2. 需求概述

- 支持PDF、Word、扫描件等多种格式
- 自动识别文档标题、段落、表格、图片等元素
- 支持自定义抽取规则
- 提供可视化标注工具
- 支持批量处理与导出

### 3. 功能列表

| 编号 | 功能模块 | 优先级 | 描述 |
|------|----------|--------|------|
| F001 | 文档上传 | P0 | 支持批量上传与格式校验 |
| F002 | 版面分析 | P0 | 识别文档布局结构 |
| F003 | 内容提取 | P1 | 抽取标题/正文/表格 |
| F004 | 数据导出 | P1 | 支持JSON/XML/CSV导出 |
| F005 | 用户管理 | P2 | 角色权限与操作审计 |

## Implementing Dynamic User-Based Visibility in Power BI

```
Current User Org Level = CALCULATE(MAX(Employees[ManagementPathLength]), FILTER(ALL(Employees),Employees[EmployeeKey] = [Current User EmployeeKey]))
```

```
Employee Row Count = COUNTROWS('Employees')
```

3. Select Manage roles in the ribbon of the Modeling tab

4. Click Create and name the role Current User, with a table filter DAX expression on the Employees table of the following:

```
PATHCONTAINS(Employees[ManagementPath],[Current User EmployeeKey])
```

5. Click the Save button

6. Create a Multi-card visualization using the Current User Name, Current User Manager, Current User Org Level, and Employee Row Count measures as Fields

7. Select View as in the ribbon of the Modeling tab and choose the Current User role

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_379_0.png)

Figure 8.12: Dynamic filter expression applied to Employees table for security role Current User

8. Edit the Current User Principal Name measure to be peter0@adventure-works.com

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_379_1.png)

Figure 8.13: Viewing the report as A. Scott Wright

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_379_2.png)

Figure 8.14: Viewing the report as Peter J. Krebs

### How it works

In a production environment, the **Current User Principal Name** measure would not be hardcoded to an email address, but would rather take the form of the following formula:

```
Current User Principal Name = USERPRINCIPALNAME()
```

The DAX functions used in this recipe are specific to a parent-child hierarchy that exists in the **DimEmployee** source table. The **DimEmployee** table contains an email address column that would correspond to the value returned by the DAX USERPRINCIPALNAME function when logged into the Power BI service. Additionally, this recipe is exclusive to Import mode datasets, as parent-child DAX functions are not currently supported in DirectQuery mode models for either calculated columns or security filter expressions.

The **Employees** table has 296 rows, but a logged-in user should only see her data and the data of those that report to her directly or indirectly. For example, a vice president should still have visibility to a manager even if the manager reports to a senior manager who reports to the vice president. The senior manager, however, should not be able to view the vice president's data or the data of a different senior manager. Visibility is limited to the current user's data and the data of those employees who report to them directly or indirectly via other managers.

The **EmployeeKey** value is the last item in the **ManagementPath** column via the PATH function. The **Current User EmployeeKey** measure, which uses the **Current User Principal Name** to retrieve the **EmployeeKey** value, is passed as the item parameter to the PATHCONTAINS function in the RLS filter expression. The calculated column created in step 1, **ManagementPath**, provides the string of values for each **Employees** row to be evaluated against.

### There's more

RLS expressions can significantly degrade query performance, as these filters will be applied in addition to other filters and expressions from Power BI reports when members of security roles access this content. As a general rule, try to use relationships between tables with low cardinality to implement dynamic security, as per the following recipe in this chapter. Utility or information functions, such as LOOKUPVALUE, CONTAINS, and PATHCONTAINS, can meet complex security rules in Import mode models but can be very expensive from a performance standpoint when applied against larger-dimension tables—such as customer or product tables containing more than one million rows.

### See also

- Row-level security (RLS) with Power BI: https://bit.ly/3nhx3zW
- Row-level security (RLS) guidance in Power BI Desktop: https://bit.ly/3tUH3kY

## Building Dynamic Security for DirectQuery

Dynamic row-level security roles can be implemented in DirectQuery models via relationships, and with specifically bidirectional cross-filtering between user security tables and the dimension tables to be secured. DAX information functions, commonly used in the role security expressions of Import mode models, such as CONTAINS and LOOKUPVALUE, are not supported in DirectQuery mode models, thus requiring a relationship-based security design. Although limited to this single approach, dynamic security can be quickly developed for DirectQuery models and maintained easily, without the need for complex DAX security expressions.

This recipe walks through the steps and settings necessary to support dynamic security in a DirectQuery model.

### Getting ready

To prepare for this recipe, follow these steps:

1. Open a Power BI Desktop file locally and access the **Power Query Editor** by clicking on **Transform Data** in the ribbon of the **Home** tab
2. Create a new query that accesses your copy of the **AdventureWorksDW2019** database on your SQL Server, ensuring that the **Data Connectivity** mode is set to **DirectQuery**
3. Choose the **FactInternetSales** table in the **Navigation** dialog
4. Rename the query **Internet Sales**
5. Right-click the **Internet Sales** query and choose **Duplicate**
6. Change the **Navigation** step to point to the **DimSalesTerritory** table
7. Rename the query **Sales Territories**
8. Right-click the **Internet Sales** query and choose **Duplicate**
9. Change the **Navigation** step to point to the **DimEmployee** table
10. Rename the query **Employees**
11. Edit the **Employees** query to be the following (replacing the **Source** line with the instance of your SQL Server):

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019"),
    dbo_DimEmployee = Source{[Schema="dbo",Item="DimEmployee"]}[Data],
    ExpandSalesTerritory = Table.ExpandRecordColumn(
        dbo_DimEmployee, "DimSalesTerritory",
        {"SalesTerritoryCountry"}, {"SalesTerritoryCountry"}),
    RemoveColumns = Table.SelectColumns(
        ExpandSalesTerritory,
        {"SalesTerritoryCountry", "EmployeeKey",
         "EmployeeNationalIDAlternateKey", "EmailAddress"})
in
    RemoveColumns
```

12. Right-click the **Employees** query and choose **Duplicate**
13. Rename the query **Countries**
14. Edit the **Countries** query to be the following (replacing the **Source** line with the instance of your SQL Server):

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019"),
    dbo_DimEmployee = Source{[Schema="dbo",Item="DimEmployee"]}[Data],
    ExpandSalesTerritory = Table.ExpandRecordColumn(
        dbo_DimEmployee, "DimSalesTerritory",
        {"SalesTerritoryCountry"}, {"SalesTerritoryCountry"}),
    RemoveColumns = Table.SelectColumns(
        ExpandSalesTerritory,{"SalesTerritoryCountry"}),
    RemoveDups = Table.Distinct(RemoveColumns)
in
    RemoveDups
```

15. Click **Close & Apply** from the ribbon of the **Home** tab
16. Switch to **Model** view and create a relationship between the **SalesTerritoryCountry** columns of the **Sales Territories** and **Countries** tables
17. Create a relationship between the **SalesTerritoryKey** columns of the **Sales Territories** and **Internet Sales** tables
18. Create a relationship between the **SalesTerritoryCountry** columns of the **Countries** and **Employees** tables
19. Save your work

### How to Build Dynamic Security Models for DirectQuery

To implement this recipe, perform the following:

1. In the **Model** view, open the properties for the **Countries** to **Employees** relationship, and set the **Cross filter direction** to **Both**
2. Check the checkbox for both **Assume referential integrity** and **Apply security filter in both directions** and then click **OK**
