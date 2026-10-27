Mkt-Sales R Squared = [Mkt-Sales Correl]^2

MktSalesSlope = DIVIDE([MktSalesCorrelNum],[MktSalesCorrelDenomX])

MktSales Intercept = AVERAGE(MktSalesRegression[Internet Sales]) - ( [MktSalesSlope]*AVERAGE(MktSalesRegression[Marketing Amount]) )

3. Click New table from the ribbon of the Modeling tab and use the following code:
Residuals =
    VAR Intercept = [MktSales Intercept]
    VAR Slope = [MktSalesSlope]
RETURN
    ADDCOLUMNS(MktSalesRegression,"Y Intercept",Intercept,"Slope",Slope,
    "Predicted Internet Sales", ([Marketing Amount]*Slope) + Intercept,
    "Residual",[Internet Sales] - (([Marketing Amount]*Slope) + Intercept))

4. Create the following measures:
Residuals Amount = SUM(Residuals[Residual])

Residuals Average = CALCULATE(AVERAGE(Residuals[Residual]),ALL(Residuals))

Residuals Sample Std Dev = CALCULATE(STDEV.S(Residuals[Residual]),ALL(Residuals))

Residuals Z Score = DIVIDE([Residuals Amount] - [Residuals Average],[Residuals Sample Std Dev])

Regression Line Message = "Regression Line: Y= " & FORMAT([MktSalesSlope],"#,###") & "X" & "+" & FORMAT([MktSales Intercept],"#,###")

Last Month Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
RETURN
    LOOKUPVALUE('Residuals'[Internet Sales],[YearMonthSort],__LastMonth)

Last Month Predicted Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
RETURN
    LOOKUPVALUE('Residuals'[Predicted Internet Sales],[YearMonthSort],__LastMonth)