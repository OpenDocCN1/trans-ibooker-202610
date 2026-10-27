
An underscore or other leading character can be used in front of variable names to ensure that a variable's name does not conflict with reserved words, and also to distinguish variables and DAX added columns from actual columns and tables in the data model.

The DISTINCT function retrieves the single value from the table for comparison with the corresponding date dimension column in the filter condition of CALCULATE. One may be tempted to use the MAX or MIN functions here in place of DISTINCT, but this will generate an error that the MAX or MIN function has been used in a True/False expression that is used as a table filter expression.

## See also

- ► DAX SELECTCOLUMNS function: https://bit.ly/2OSw4JA
- ► DAX LOOKUPVALUE function: https://bit.ly/3gdjZK1
- ► DAX ROW function: https://bit.ly/3dobQ3R

## Leveraging Calculation Groups

When working with date intelligence, it is common to have many quasi-redundant measures that calculate various different metrics for YTD, MTD, PY, PYTD, PMTD, and other common date intelligence intervals. For example, business users may be interested in a year-to-date, year-over-year, and year-over-year percentage calculation for the gross sales, net sales, and other measures. Normally, this would require nine DAX measures to calculate these measures at each of those date intelligence intervals.

Calculation groups can greatly assist in reducing the number of measures required by allowing a DAX expression to be reused for any measure. In the example given, this means that three DAX expressions could serve the needs of the nine DAX expressions required without the use of calculation groups.

This recipe demonstrates the use of calculation groups in the context of date intelligence in order to eliminate redundant DAX expressions.

## Getting ready

To prepare for this recipe, follow these steps:

- 1. Open Power BI Desktop.
- 2. Create an Import mode data source query called AdWorksDw. This query should be similar to the following:

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV",
"AdventureWorksDW2019")
in
Source
```

- 3. Isolate this query in a query group called Data Sources and disable loading.
- 4. Right-click AdworksDW and choose Reference.
- 5. Select the FactInternetSales table and name this query Internet Sales.
- 6. Repeat steps 4 and 5 for the DimDate table and name this query Order Dates.
- 7. Separate the fact query, Internet Sales, into a Facts query group.
- 8. Separate the dimension query, Order Dates, into a Dimensions query group.
- 9. Create a Blank Query called Calculations in the Other Queries group.
- 10. Select Close & Apply in the ribbon of the Home tab to load the tables to the data model.
- 11. Create a relationship using the FullDateAlternateKey column from the Order Dates table and the OrderDate column of the Internet Sales table.
- 12. Rename the FullDateAlternateKey column to Date.
- 13. Mark the Order Dates table as a date table using Date as the Date column.
- 14. In the Calculations table, create some simple measures:
- 15. Hide the Calculations column in the Calculations table.
- 16. Set the Sort by column for the EnglishMonthName column to the MonthNumberOfYear column.

```
Internet Sales = SUM('Internet Sales'[SalesAmount])
```

```
Orders = COUNTROWS('Internet Sales')
```

## How to Leverage Calculation Groups

To implement this recipe, carry out the following steps:

- 1. Select Tabular Editor from the ribbon of the External Tools tab
- 2. In Tabular Editor, select Model and then New Calculation Group
- 3. Name the new calculation group Date Intelligence Group
- 4. Expand Date Intelligence Group, right-click Calculation Items, and choose New Calculation Item
5. Create a current calculation item with the formula SELECTEDMEASURE()

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_324_0.png)

Figure 6.8: Creation of the Current calculation item

6. Repeat steps 4 and 5 to create the following calculation items:

For YTD:

```
CALCULATE(SELECTEDMEASURE(),
    FILTER(ALL('Order Dates'),
        'Order Dates'[CalendarYear] = MAX('Order Dates'[CalendarYear])
        &&
        'Order Dates'[Date] <= MAX('Order Dates'[Date])))
```

For MTD:

```
CALCULATE(SELECTEDMEASURE(),
    FILTER(ALL('Order Dates'),
        'Order Dates'[CalendarYear] = MAX('Order Dates'[CalendarYear])
        &&
        'Order Dates'[MonthNumberOfYear] = MAX('Order Dates'[MonthNumberOfYear])
        && 'Order Dates'[Date] <= MAX('Order Dates'[Date])))
```

For QTD:

```
CALCULATE(SELECTEDMEASURE(),
    FILTER(ALL('Order Dates'),
        'Order Dates'[CalendarYear] = MAX('Order Dates'[CalendarYear])
        &&
        'Order Dates'[CalendarQuarter] = MAX('Order Dates'[CalendarQuarter]) &&
        'Order Dates'[Date] <= MAX('Order Dates'[Date])))
```

For PY:

```
VAR __MaxDate = MAX('Order Dates'[Date])
VAR __MinDate = MIN('Order Dates'[Date])
RETURN
    CALCULATE(SELECTEDMEASURE(),
FILTER(ALL('Order Dates'),
    'Order Dates'[Date] >=
        DATE(YEAR(__MinDate)-1,MONTH(__MinDate),DAY(__MinDate))
    &&
        'Order Dates'[Date] <=
        DATE(YEAR(__MaxDate)-1,MONTH(__MaxDate),DAY(__MaxDate)))
```
For PY YTD:
VAR __MaxDate = MAX('Order Dates'[Date])
RETURN
    CALCULATE(SELECTEDMEASURE(),
        FILTER(ALL('Order Dates'),
            'Order Dates'[CalendarYear] =
                MAX('Order Dates'[CalendarYear])-1 &&
            'Order Dates'[Date] <=
                DATE(
                    YEAR(__MaxDate)-1,
                    MONTH(__MaxDate),
                    DAY(__MaxDate))))
For YOY:
SELECTEDMEASURE()
    - CALCULATE(SELECTEDMEASURE(),'Date Intelligence Group'[Name] = "PY")
For YOY %:
DIVIDE(
    CALCULATE(SELECTEDMEASURE(),'Date Intelligence Group'[Name] = "YOY"),
    CALCULATE(SELECTEDMEASURE(),'Date Intelligence Group'[Name] = "PY"))
```

- 7. For the YOY % calculation item, set the Format String Expression to `0.00%;-0.00%;0.00%`
- 8. In Tabular Editor, select File and then Save
- 9. In Power BI Desktop, click Refresh now in the warning box regarding refreshing calculation groups

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_325_0.png)

Figure 6.9: Manually refreshing calculation groups

- 10. Create a Matrix visualization with the CalendarYear and EnglishMonthName columns from the Order Dates table as Rows, the Internet Sales measure as Values, and the Name column from the Date Intelligence Group as Columns
- 11. Add the Orders measure to the Matrix visualization as Values

## How it works...

Currently, Power BI Desktop does not provide a method for creating and editing calculation groups, so this must be done in Visual Studio or Tabular Editor.

The SELECTEDMEASURE function takes the current measure in context and replaces the values returned by the measure with the calculations from the calculation group. The format of the value returned (Currency, Whole Number, Decimal) corresponds with the format of the selected measure, unless overridden by a specified Format String Expression. For numeric values, the Format String Expression should always include three format specifications separated by semi-colons for positive, negative, and zero values.

The order of the calculation items returned will be alphabetical when the Ordinal property is set to -1 for all calculation items. This order can be overridden by specifying Ordinal numbers from 0 and higher. Lower Ordinal numbers display first.

## There's more...

The same results can be achieved using DAX time intelligence functions with the caveat that DAX time intelligence functions assume a standard (Gregorian) calendar, and thus should be avoided when dealing with fiscal calendars and more complex scenarios. To see how this works, do the following:

- 1. In Tabular Editor, select Model and then New Calculation Group
- 2. Name the new calculation group Time Intelligence Group
- 3. Create the following calculation items:
  - **Current:** `SELECTEDMEASURE()`
  - **YTD:** `CALCULATE(SELECTEDMEASURE(), DATESYTD('Order Dates'[Date]))`
  - **MTD:** `CALCULATE(SELECTEDMEASURE(), DATESMTD('Order Dates'[Date]))`
  - **QTD:** `CALCULATE(SELECTEDMEASURE(), DATESQTD('Order Dates'[Date]))`
  - **PY:** `CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR('Order Dates'[Date]))`
  - **PY YTD:** `CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR('Order Dates'[Date]), 'Time Intelligence Group'[Name] = "YTD")`
  - **YOY:** `SELECTEDMEASURE() - CALCULATE(SELECTEDMEASURE(), 'Time Intelligence Group'[Name] = "PY")`
  - **YOY %:** `DIVIDE(CALCULATE(SELECTEDMEASURE(), 'Time Intelligence Group'[Name] = "YOY"), CALCULATE(SELECTEDMEASURE(), 'Time Intelligence Group'[Name] = "PY"))`
