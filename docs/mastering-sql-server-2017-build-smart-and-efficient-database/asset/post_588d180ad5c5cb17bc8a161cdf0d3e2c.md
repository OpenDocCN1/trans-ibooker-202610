
+   12. When setting up the columns correctly, click the **Script** tab and then the **Edit Script** button to open the VSTA environment.
+   13. In the script, find the `Input0_ProcessInputRow` method. Add the following two variables just before the method definition and after the comments, with the summary and parameters description of this method:
+   14. Save the code end exit the VSTA environment. In the Script Transformation Editor, click on **OK**.
+   15. Add a **Multicast** transformation to the data flow and connect it with the blue arrow from the Script component. This transformation serves as a placeholder only, in order to enable a Data Viewer after the Script transformation does its job.

```
Double MyDate;
Double MyOrderId;
Add the following code to the method:
// Check if OrderId column is null
if (Row.OrderId_IsNull)
{
    // if null then replace value with variable value
    Row.OrderId = MyOrderId;
}
else
{
    // if not null then replace variable value with column value
    MyOrderId = Row.OrderId;
}
// Check if Date column is null
if (Row.Date_IsNull)
{
    // if null then replace value with variable value
    Row.Date = MyDate;
}
else
{
    // if not null then replace variable value with column value
    MyDate = Row.Date;
}
```

16. Enable two Data Viewers, one on the path from the source to the Script component, and one from the Script component to the Multicast transformation. Your data flow should look as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_514_0.png)

17. Note that, depending on the Excel version you are using, you might need to change the `Run64BitRuntime` property of your project to False before running the package, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_515_0.png)

18. Save the package and run it. Observe the data before correction with the Script component and after it. When finished, stop debugging.

## Validating data using regular expressions in a Script component

In this recipe, you will create a Script Component that will use the regular expressions in the `DataProfiling.xml` file to validate the emails of the personal data extracted from a flat file.

## Getting ready

This recipe assumes that you successfully completed the *Reading data profiling XML results with Script task* recipe earlier in this chapter. If you did not complete that recipe, you can prepare an appropriate SSIS package simply by completing step 1 of that recipe.

## How to do it...

+   1. Make sure that the `RegExValidation.dtsx` package is active in the control flow designer.
+   2. Right-click in the empty canvas of the **Connection Managers** pane at the bottom of the SSDT window and select **New Flat File Connection...** from the shortcut menu.
+   3. Use the **Flat File Connection Manager Editor** to create a new connection manager, and on the **General** page enter `PersonData` into the Connection manager name textbox.
+   4. To determine the **File name** property, click **Browse...**, and then select the `Ch11_PersonData.csv` file in the `C:\SSIS2016Cookbook\Chapter11\Files\` folder.
+   The file should be recognized as a Unicode file automatically; otherwise, check the **Unicode** option below the **Browse...** button.
+   5. Leave the rest of the properties unchanged, as shown in the following screenshot:

6. On the **Columns** page, verify that all the source columns have been recognized: Title, FirstName, MiddleName, LastName, Suffix, and EmailAddress.

If any of the columns are missing, click **Reset Columns** below the columns list.

7. Click on **OK** to complete the configuration.

+   15. Click **OK** to confirm the component type, and then change its name to Validate Email.
+   16. Connect the regular data path from the **Person Data** source to the **Validate Email** transformation.
+   17. Double-click the **Validate Email** transformation to open the Script Task Editor.
+   18. On the Script page, add the **$Package::DataProfileName** package parameter to the **ReadOnlyVariables** collection, and make sure that Microsoft Visual C# 2015 is selected as the **ScriptLanguage** property, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_519_0.png)

+   19. On the **Input Columns** page, make sure that all the input columns are selected; their usage type should be **ReadOnly**.
+   20. On the **Inputs and Outputs** page, in the **Inputs and outputs** list, select the **Input 0** input, and in the properties pane on the right-hand side of the editor, change its **Name** property to `PersonDataInput`.
+   21. Select the **Output 0** output, and change its name to `PersonDataOutput`.
+   22. In the `PersonDataOutput` properties, locate the `SynchronousInputID` property and make sure it references the `Validate Email.Inputs[PersonDataInput]` input.
+   This means that the component's output is synchronous.
+   23. Expand the `PersonDataOutput` in the **Inputs and outputs** list and select the Output Columns node.
+   24. Click on **Add Column** to create a column in the output; use the following settings to configure it:
+   Property Value
+   Name IsValidEmail
+   DataType Boolean [DT_BOOL]
+   25. Click on **OK** to confirm the component's configuration so far.
+   It is recommended to save all Script component settings, such as variables, inputs, outputs, and additional connection managers, before creating the script.
+   This way, if there are problems with the script that you cannot resolve during editing, the rest of the settings do not need to be set again.
+   26. Open the **Validate Email** transformation editor again, and on the Script page click **Edit Script...** to start **Visual Studio for Applications (VSTA)** IDE.
+   27. Locate the `Namespaces` region at the beginning of the component definition and add the following references to the region:
+   using System.Collections.Generic;
+   using System.Text.RegularExpressions;
+   using System.Xml;

![](img/07d1225e179d5df9f2b2d26b026386c6_520_0.png)

28. In the `ScriptMain` class definition, add the following constant and private variable declarations:

```
private const String DATA_PROFILE_NAMESPACE =
    "http://schemas.microsoft.com/sqlserver/2008/DataDebugger/";
private const String DATA_PROFILE_NAMESPACE_ALIAS = "dp";

private String _regexElementXPath =
    "/dp:DataProfile/dp:DataProfileOutput/dp:Profiles" +
    "/dp:ColumnPatternProfile[dp:Column[@Name='EmailAddress']]" +
    "/dp:TopRegexPatterns/dp:PatternDistributionItem/dp:RegexText/text()";

private List<String> _regexPatterns = new List<String>();
```

Typically, variable definitions are placed at the beginning of the class definition, right after the three Help regions.

29. At the end of the `ScriptMain` class definition (before the last closing brace) place the following function definition:

```
private List<String> LoadRegularExpressions(String dataProfileName)
{
    List<String> result = new List<String>();
    XmlDocument dataProfile = new XmlDocument();
    dataProfile.Load(dataProfileName);
    XmlNamespaceManager dataProfileNS = new
    XmlNamespaceManager(dataProfile.NameTable);
    dataProfileNS.AddNamespace(DATA_PROFILE_NAMESPACE_ALIAS,
    DATA_PROFILE_NAMESPACE);

    foreach (XmlNode regexPatternElement in
    dataProfile.SelectNodes(_regexElementXPath, dataProfileNS))
    {
        String regexPattern = regexPatternElement.Value;
        if (!result.Contains(regexPattern))
        {
            result.Add(regexPattern);
        }
    }

    return result;
}
```

This function extracts all relevant regular expression patterns from the data profiling result and loads them into a variable.

+   30. Right after the LoadRegularExpressions() function, add another function:

```
private Boolean IsValidEmail(String emailAddress)
{
    Boolean result = false;
    if (!String.IsNullOrEmpty(emailAddress))
    {
        foreach (String regexPattern in _regexPatterns)
        {
            if (Regex.IsMatch(emailAddress, regexPattern, RegexOptions.IgnoreCase))
            {
                result = true;
                break;
            }
        }
    }
    return result;
}
```

This function tests the supplied email address against all available regular expressions, and returns true as soon as one of the patterns is matched; otherwise, it returns false.

+   31. Locate the PreExecute() method add the following command to its definition:

```
_regexPatterns.AddRange(this.LoadRegularExpressions(Variables.DataProfileName));
```

The PreExecute() data flow component method is called at the beginning of the data flow execution, before the rows are acquired from the upstream pipeline. As the same set of regular expressions is going to be used throughout the data flow execution, it only needs to be loaded once, not for every row.
