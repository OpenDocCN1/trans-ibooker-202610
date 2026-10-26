![](bbox=[0.239, 0.106, 0.761, 0.514])

+   7. Click on **OK** to confirm the selection.
+   8. Change the name of the Script component to Airport Info by Country.
+   9. Double-click on the **Airport Info by Country** source to open the Script Transformation Editor.
+   10. On the **Script** page, add the `$Package::CountryName` parameter to the `ReadOnlyVariables` collection, and make sure that Microsoft Visual C# 2015 is selected as the **ScriptLanguage** property.
+   11. On the **Inputs and Outputs** page, select **Output 0** and change its name to `AirportInfoOutput`.
+   12. Add columns to the output's Column Collection using the following settings:

```
Name DataType Length
Country Unicode string [DT_WSTR] 50
AirportName Unicode string [DT_WSTR] 50
AirportCode Unicode string [DT_WSTR] 3
RunwayLengthFeet four-byte signed integer [DT_I4] /
```