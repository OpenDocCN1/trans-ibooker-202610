
## Bike Sales

| Date | Sum of Sales | Sales - null to 0 | Profit Margin Bike Sales |
|---|---|---|---|
| 1-6-25 | 286.639,50 | 286.639,50 | 32,9% |
| Canada | 80.200,80 | 80.200,80 | 29,1% |
| France | 65.340,00 | 65.340,00 | 33,3% |
| Germany |  | 0,00 |  |
| Mexico | 74.100,00 | 74.100,00 | 33,3% |
| United States of America | 66.998,70 | 66.998,70 | 23,4% |
| Total | 286.639,50 | 286.639,50 | 32,9% |

Figure 6-15. The result of a DIVIDE measure

If [Profit Bikes] is also blank when [Total Sales] is blank (which is usually the case), then the whole expression stays blank. Even if it’s not blank, users often understand that dividing by 0 gives either a blank or “infinity.”

So, you may think, why does it matter whether or not you use a 0 value? The answer is that using a 0 value doesn’t always go smoothly. It can influence your results and show incorrect findings. For example, let’s look at these two AVERAGEX calculations:

```
AverageX Bike Sales = AVERAGEX(Sales,[Total Sales])
```

```
AverageX Bike Sales - 0 value = AVERAGEX(Sales,[Sales - null to 0])
```

The difference between them is in how they treat blanks:

AVERAGEX(Sales, [Total Sales])
This skips blanks and ignores Germany.

AVERAGEX(Sales, [Sales - null to 0])
This includes Germany as 0.

If you replace a blank with 0, then suddenly, you’re calculating something completely different. This can seriously mess up key performance indicators (KPIs), dashboards, and anything else in which precision matters (as shown in Figure 6-16).

Bike Sales
Date | Sum of Sales | Sales - null to 0 | AverageX Bike Sales | AverageX Bike Sales - 0 value
1-6-25
Canada | 80.200,80 | 80.200,80 | 40.100,40 | 40.100,40
France | 65.340,00 | 65.340,00 | 32.670,00 | 32.670,00
Germany |  | 0,00 |  | 0,00
Mexico | 74.100,00 | 74.100,00 | 37.050,00 | 37.050,00
United States of America | 66.998,70 | 66.998,70 | 33.499,35 | 33.499,35
Total | 286.639,50 | 286.639,50 | 35.829,94 | 28.663,95

Figure 6-16. Table showing how blanks and 0 are treated in a measure

Showing a 0 might seem cleaner, but a blank and a 0 don’t mean the same thing:

### A blank

This means no data—maybe nothing was sold, the product doesn’t exist, or the data hasn’t been collected yet.

### 0

This means a real value of zero.

Replacing blank values with 0 might seem like a harmless change that enhances your report’s appearance. However, once you begin using these 0 values in calculations, problems can quickly arise. AVERAGEX, for instance, treats blanks and zeros very differently. While it ignores blanks in the calculation, it counts zeroes and thus pulls down your average and potentially misrepresents your data. This can impact how users interpret KPIs.

There’s also a performance aspect to consider. Using an expression like IF(ISBLANK(), 0) introduces additional logic that the Power BI engine must evaluate for each row. Especially within iterator functions like AVERAGEX and SUMX, Power BI evaluates that branching repeatedly, row by row. While this may not matter in a small model, it becomes significant in real-world datasets with millions of rows, as it can slow down performance.

## Using DIVIDE Instead of the / Operator

When report creators start using Power BI and DAX, they often use the slash (/) operator to divide one value by another. It feels intuitive, and Power BI allows it without complaint.

However, there’s also the DIVIDE function, which offers important benefits over the / operator. It not only optimizes for better performance, but it also handles blank values efficiently (see Figure 6-17).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_328_0.png)

Also, DIVIDE has an optional third argument that enables you to define an alternative result when the division can’t be performed (for instance, when the denominator is a NULL value):

```
DIVIDE(<numerator>, <denominator> [, <alternateresult>])
```

Without DIVIDE, you’d need to write additional IF logic to manage such cases, and that requires more writing effort from you and more time for Power BI to evaluate.

Let’s look at our bike sales table in Figure 6-18 again. Note that it has a NULL value in the Sales column:

## Bike Sales

| Date | Country | Sales | Profit |
|---|---|---|---|
| 1-6-25 | Canada | 80.200,80 | 23.320,80 |
| 1-6-25 | France | 65.340,00 | 21.780,00 |
| 1-6-25 | Germany |  | 8.880,00 |
| 1-6-25 | Mexico | 74.100,00 | 24.700,00 |
| 1-6-25 | United States of America | 66.998,70 | 15.658,70 |
| Total |  | 286.639,50 | 94.339,50 |

Figure 6-18. Table with a NULL value

Our sales dataset has data by date and country, showing both sales and profit. To calculate the profit margin, we divide profit by total sales, and there are two ways to do this in DAX: use the standard slash (/) operator or use the DIVIDE function. The DIVIDE function is more robust, especially when we’re dealing with missing or zero values, as in our example.

We’ve created two DAX measures to show the effect of dividing the profit (the numerator) by the sales (the denominator). In this initial example, we don’t provide an alternative result for the DIVIDE function to show you how NULL values are handled:

```
Margin / Operator = [Profit Bikes]/Sales[Total Sales]
```

```
Margin DIVIDE = Divide([Profit Bikes],Sales[Total Sales])
```

When we compare the two approaches, the difference between them becomes clear. Using the slash operator results in “infinity” when total sales are blank, because Power BI can’t properly handle that scenario. With the DIVIDE function, however, Power BI returns a blank value as expected, because no alternate result was provided. This makes the DIVIDE function the safer and more predictable option for handling division in your DAX measures (see Figure 6-19).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_330_0.png)

Figure 6-19. Preventing “infinity” by using a DIVIDE statement

Of course, you can also provide an alternate result for cases in which the division isn’t possible. This result can be any value you want, such as 0, a dash (“-”), or even custom text. You add it as the third argument in the DIVIDE function, like this:

```
Margin DIVIDE - = Divide([Profit Bikes],Sales[Total Sales],"-")
```

This way, instead of returning a blank when the denominator is 0 or absent, Power BI will display your chosen value in the result (see Figure 6-20).

## Bike Sales

| Date | Country | Sales | Profit | Margin / Operator | Margin DIVIDE |
|---|---|---|---|---|---|
| 1-6-25 | Canada | 80.200,80 | 23.320,80 | 29,1% | 29,1% |
| 1-6-25 | France | 65.340,00 | 21.780,00 | 33,3% | 33,3% |
| 1-6-25 | Germany |  | 8.880,00 | Infinity | - |
| 1-6-25 | Mexico | 74.100,00 | 24.700,00 | 33,3% | 33,3% |
| 1-6-25 | United States of America | 66.998,70 | 15.658,70 | 23,4% | 23,4% |
| Total |  | 286.639,50 | 94.339,50 | 32,9% | 32,9% |

Figure 6-20. Displaying a chosen value instead of a blank

Using the DIVIDE function is better than using the / operator in DAX. While both calculate the same result, DIVIDE is more reliable; it handles division by zero or blank values without returning “infinity” as a result, and you can even choose what should be returned in such cases. Moreover, it improves performance by avoiding the need for extra IF statements to manage exceptions. Additionally, DIVIDE makes your DAX code easier to read and maintain, especially when you’re working with more complex measures.

## Are IFERROR and ISERROR as Helpful as They Look?

If you’re experienced with Excel, you’re probably familiar with IFERROR and ISERROR. They seem like handy tools: they catch errors, return a fallback value, and are done. However, in Power BI, these functions can slow things down, especially in larger models or more complex reports.

The DAX for the two tools look like this:

```
ISERROR = ISERROR(<value>)
IFERROR = IFERROR(value, value_if_error)
```

The difference between them is that ISERROR returns either TRUE (there’s an error) or FALSE (if there’s no error), while IFERROR evaluates whether there’s an error and then provides an alternative value for that error. While all of that sounds helpful, the issue is that, even when there is no error, the whole logic is executed to check for any error, and that adds performance time.

Also, while the function may seem helpful sometimes, we may have other logic that works better. Let’s look at this example based on our bike sales margin, where one column returns a NULL value:

```
ISERROR = ISERROR([Margin / Operator])
IFERROR = IFERROR([Margin / Operator], BLANK())
```

The ISERROR statement will return a TRUE value when we have a NULL value, and the IFERROR measure will return a blank value when we find the NULL value. But if you revisit the previous section on the DIVIDE function, you may realize that DIVIDE would be a great option for this, too, because it automatically returns a blank value when no value can be returned:

```
Margin DIVIDE = DIVIDE([Profit Bikes], Sales[Total Sales])
```

Figure 6-21 shows the difference between handling a blank value with ISERROR and doing it with IFERROR.
