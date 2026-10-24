# Custom text transform on column FundingAmount

Expression

Language General Refine Expression Language (GREL)

```
value.replace(',', '')
```

No syntax error.

3. Now, repeat the previous step, but instead of a comma, remove the $ character by typing a different expression: value.replace('$', ''), confirm the formula, and click OK.

4. In steps 2 and 3, we replaced text (or string) values with other text values, making OpenRefine think this column is no longer numeric. As a result, all values are once again left-aligned and in black. Perform step 1 again. This time, nearly all of the cells will have turned green, meaning they successfully converted to numeric. However, a few nonnumeric black cells remain.

5. To fix the remaining nonnumeric black cells, we need to remove spaces and an a character at the end of one number. Fix these manually by hovering over a cell, click Edit, and in the new pop-up window, change “Data type” to “number,” and press Apply.