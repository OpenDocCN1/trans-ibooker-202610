
## Accessing and Retrieving Data

Uncheck the **(Select All)** option to deselect all values and then check the box next to a country, such as the **United States**, before clicking the **OK** button.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_48_0.png)

Figure 2.2: Filtering for United States only in the Query Editor

Note that the button for the **EnglishCountryRegionName** column changes to display a funnel icon. Also notice that, in the **Query Settings** pane on the right side of the window, a new option under **APPLIED STEPS** has appeared called **Filtered Rows**.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_48_1.png)

Figure 2.3: The Query Settings pane in the Query Editor

### How to View and Analyze M Functions

There are two methods for viewing and analyzing the M functions comprising a query; they are as follows:

- Formula bar
- Advanced Editor

The formula bar exposes the M function for the current step only. This formula bar appears just above the column headers for the preview data in the central part of the window. If you do not see this formula bar, click the **View** tab and check the box next to **Formula Bar** in the **Layout** section of the ribbon. All such areas of interest are boxed in red in *Figure 2.4*.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_49_0.png)

*Figure 2.4: The Power Query Editor formula bar*

When the **Source** step is selected under **APPLIED STEPS** in the **Query Settings** pane, as seen in *Figure 2.3*, we see the connection information specified on the initial dialog after selecting **Get Data** and then **SQL Server**. The M function being used is `Sql.Database`. This function is accepting two parameters: the server name, `localhost\MSSQLSERVERDEV`, and the database name, AdventureWorksDW2019. Clicking on other steps under **APPLIED STEPS** exposes the formulas for those steps, which are technically individual M expressions.

The formula bar is useful to quickly understand the M code behind a particular query step. However, it is more convenient and often essential to view and edit all the expressions in a centralized window. This is the purpose of the **Advanced Editor**. To launch the **Advanced Editor**, follow these steps:

1. Click on the **Home** tab and then select **Advanced Editor** from the **Query** section of the ribbon, as shown in *Figure 2.5*. Alternatively, the **Advanced Editor** can also be accessed from the **View** tab, shown in *Figure 2.4*.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_50_0.png)

Figure 2.5: Advanced Editor on the Home tab of the Query Editor

2. The Advanced Editor dialog is displayed, exposing all M functions and comments that comprise the query. The M code can be directly edited from within this dialog.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_50_1.png)

Figure 2.6: The Advanced Editor view of the DimGeography query

As shown in Figure 2.6, using the Advanced Editor will mean that all of the Power Query code that comprises the query can be viewed in one place.

### How it works

The majority of queries created for Power BI follow the let...in structure, as per this recipe. Within the let block, there are multiple steps with dependencies among those steps. For example, the second step, dbo_DimGeography, references the previous step, Source. Individual expressions are separated by commas, and the expression referred to following the in keyword is the expression returned by the query. The individual step expressions are technically known as "variables".

Variable names in M expressions cannot have spaces without being preceded by a hash sign and enclosed in double quotes. When the Query Editor graphical interface is used to create M queries, this syntax is applied automatically, along with a name describing the M transformation applied. This behavior can be seen in the Filtered Rows step in Figure 2.6. Applying short, descriptive variable names (with no spaces) improves the readability of M queries.

Note the three lines below the `let` statement. These three lines correspond to the three **APPLIED STEPS** in our query: **Source**, **Navigation**, and **Filtered Rows**. The query returns the information from the last step of the query, **Filtered Rows**. As more steps are applied, these steps will be inserted above the `in` statement and the line below this will change to reference the final step in the query.

M is a case-sensitive language. This includes referencing variables in M expressions (`RenameColumns` versus `Renamecolumns`) as well as the values in M queries. For example, the values "Apple" and "apple" are considered unique values in an M query.

It is recommended to use the Power Query Editor user interface when getting started with a new query and when learning the M language. After several steps have been applied, use **Advanced Editor** to review and optionally enhance or customize the M query. As a rich, functional programming language, there are many M functions and optional parameters not exposed via the Power Query Editor's graphical user interface. Going beyond the limits of the Power Query Editor enables more robust data retrieval, integration, and data mashup processes.

The M engine also has powerful "lazy evaluation" logic for ignoring any redundant or unnecessary variables, as well as short-circuiting evaluation (computation) once a result is determinate, such as when one side (operand) of an OR logical operator is computed as True. Lazy evaluation allows the M query engine to reduce the required resources for a given query by ignoring any unnecessary or redundant steps (variables). The order of evaluation of the expressions is determined at runtime—it doesn't have to be sequential from top to bottom.

In the following example, presented in *Figure 2.7*, a step for retrieving Canada was added and the "Filtered Rows" step for filtering the results for the United States was ignored. Since the CanadaOnly variable satisfies the overall `let` expression of the query, only the Canada query is issued to the server as if the "Filtered Rows" step were commented out or omitted.

```
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019"),
    dbo_DimGeography = Source{[Schema="dbo",Item="DimGeography"]}[Data],
    CanadaOnly = Table.SelectRows(dbo_DimGeography, each ([EnglishCountryRegionName] = "Canada")),
    #"Filtered Rows" = Table.SelectRows(dbo_DimGeography, each ([EnglishCountryRegionName] = "United States"))
in
    CanadaOnly
```

Figure 2.7: Revised query that ignores the "Filtered Rows" step to evaluate Canada only

As a review of the concepts covered thus far and for future reference, *Table 2.1* presents a glossary of the main concepts of the M language utilized in this book.

| Term | Definition |
| --- | --- |
| Expression | Formulas evaluated to yield a single value. Expressions can reference other values, such as functions, and may include operators. |
| Value | The result of evaluating an expression. Values can be categorized into types which are either primitive, such as text ("abc"), or structured kinds, such as tables and lists. |
| Function | A value that produces a new one based on the mapping of input values to the parameters of the function. Functions can be invoked by passing parameter values. |
| Type | A value that classifies other values. The structure and behavior of values are restricted based on the classification of their type, such as Record, List, or Table. |
| `let` | An expression that allows a set of unique expressions to be assigned names (variables) and evaluated (if necessary) when evaluating the expression following the `in` expression in a `let...in` construct. |
| Variable | A unique, named expression within an environment to be conditionally evaluated. Variables are represented as Applied Steps in the Query Editor. |
| Environment | A set of variables to be evaluated. The global environment containing the M library is exposed to root expressions. |
| Evaluation | The computation of expressions. Lazy evaluation is applied to expressions defined within let expressions; evaluation occurs only if needed. |
| Operators | A set of symbols used in expressions to define the computation. The evaluation of operators depends on the values to be operated on. |

Table 2.1: M Language elements

### There's more...

M queries are not intended as a substitute for the data loading and transformation workloads typically handled by enterprise data integration and orchestration tools such as **Azure Data Factory (ADF)**, Azure Databricks, or **SQL Server Integration Services (SSIS)**. However, just as BI professionals carefully review the logic and test the performance of SQL stored procedures and ETL packages supporting their various cubes and reporting environments, they should also review the M queries created to support Power BI models and reports. When developing retrieval processes for Power BI models, consider these common ETL questions:

- How are queries impacting the source systems?
- Can we make our queries more resilient to changes in source data so that they avoid failure?
- Are our queries efficient and simple to follow and support, or are there unnecessary steps and queries?
- Are our queries delivering sufficient performance to the BI application?
- Is our process flexible, such that we can quickly apply changes to data sources and logic?
- Can some or all of the required transformation logic be implemented in a source system such as the loading process for a data warehouse table or a SQL view?
