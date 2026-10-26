
+   32. Locate the `PersonDataInput_ProcessInputRow()` method, and add the following command to its definition:
+   
+   ```csharp
+   Row.IsValidEmail = this.IsValidEmail(Row.EmailAddress);
+   ```
+   
+   The `IsValidEmail()` function needs to be called for each row as it needs to validate each e-mail address extracted from the source.

+   33. Save the VSTA project and build it by selecting the **Build** command from the **Build** menu.
+   
+   If you entered all the code correctly, the build should succeed; otherwise, you need to follow the error messages in the Error pane to resolve the errors.

+   34. After the successful build, close the VSTA window, and return to SSDT.

+   35. In the Script Transformation Editor, click on **OK**, to confirm and save the component script, and then save the package.

+   36. From the SSIS Toolbox, drag a **Conditional Split** transformation to the data flow designer, and change its name to `Valid or Invalid Email`.

+   37. Connect the regular data path from the **Validate Email** transformation to the **Conditional Split** transformation.

+   38. Double-click the newly added transformation to open the **Conditional Split Transformation Editor**.

+   39. Create a new output using the following settings:
+   
+   - **Property:** `Value`
+   - **Output name:** `Valid Email`
+   - **Condition:** `IsValidEmail`
+   
+   Alternatively, to reduce typing, you can drag the `IsValidEmail` column from the **Columns** list in the top-left part of the **Conditional Split Transformation Editor**.

40. Change the Default output name to Invalid Email.

Refer to the following screenshot to verify your settings:

![](img/07d1225e179d5df9f2b2d26b026386c6_524_0.png)

+   41. Click on **OK** to complete the configuration.
+   42. From the SSIS Toolbox, drag a **Multicast** transformation to the data flow designer and connect the regular data path from the **Valid** or **Invalid Email** transformation to it.
+   43. When prompted by the **Input Output Selection** dialog, select the **Invalid Email** in the **Output** selection box, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_525_0.png)

+   44. Click on **OK** to confirm the selection.
+   45. Enable the **Data Viewer** on the **Invalid Email** data path.
+   46. Save the package and execute it in debug mode. Observe the execution and inspect the rows placed in the **Invalid Email** data path.
+   47. Stop the debug mode execution; leave the solution open, as you will need it for the next recipe.

## Using the Script component as a source

In this recipe, you will create a custom source by using the Script Component as a data flow source.

You will connect to a web service and retrieve the data from it, which you will then place into the data flow pipeline.

### How to do it...

+   1. Add a new SSIS package to the **AdventureWorksETL** project you created at the beginning of this chapter.
+   2. Change the name of the newly created package to `CustomWebServiceSource.dtsx` and save it.
+   3. Make sure the **CustomWebServiceSource.dtsx** package is active, and then create a new package parameter using the following settings:
      - **Property:** `Value`
      - **Name:** `CountryName`
      - **Data type:** `String`
      - **Value:** `France`
+   4. Drag a **Data Flow** task to the control flow designer and change its name to Airport Information.
+   5. Open the **Airport Information** task in the data flow designer and drag a **Script Component** to the data flow designer.
+   6. In the **Select Script Component Type** dialog, select **Source**, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_527_0.png)

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

Refer to the following screenshot to verify your settings:

![](img/07d1225e179d5df9f2b2d26b026386c6_528_0.png)

💡 **TIP**

To ensure that each new column is added to the end of the list, click on the **Output Columns** node before adding the next column.

+   13. Click OK to confirm the configuration.
+   14. Open the **Airport Info by Country** transformation editor again, and this time click **Edit Script...** to start VSTA.

+   15. In the **Solution Explorer**, right-click the **References** node and select **Add Service Reference...** from the shortcut menu, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_529_0.png)

+   16. In the **Add Service Reference** dialog, click on **Advanced...** on the bottom-left of the dialog.
+   
+   17. In the **Service Reference Settings** dialog, click on **Add Web Reference...** to open the **Add Web Reference** dialog.
+   
+   18. Enter the following URL in the **URL** textbox:

```
http://www.webservicex.net/airport.asmx?WSDL
```

+   19. Click the arrow pointing to the right, on the right-hand side of the textbox, to load the information from the web service.

20. Enter AirportInfo into the Web reference name textbox.

Refer to the following screenshot to verify your settings:

![](img/07d1225e179d5df9f2b2d26b026386c6_530_0.png)

21. Click Add Reference to confirm the settings and create the reference.

22. Save the project.

23. In the component definition, at the beginning of the code, locate the Namespaces region and expand it to list the existing references.

24. Add another using command after the last one, and start typing the name of the project (it should be similar to SC_04df03808e054a509e49d4cce022812c); allow the Visual Studio intellisense to suggest the complete namespace, and then complete the reference to the AirportInfo namespace:

```
using SC_04df03808e054a509e49d4cce022812c.AirportInfo;
```

+   25. The `SC_04df03808e054a509e49d4cce022812c` value in this example is used for the name of the VSTA project and for its default namespace. When you add a web reference to the project, proxy classes, and functions are created automatically and placed in the project's default namespace.
+   
+   26. Add the following reference as it will be needed later:
+   
+   ```csharp
+   using System.Xml;
+   ```
+   
+   27. From the **Build** menu, select **Build**, to save and build the project.
+   
+   28. If you followed the preceding steps correctly, the project should build successfully; otherwise, inspect the errors reported in the Error pane and make the necessary corrections.
+   
+       1. If errors are returned from the automatically generated code, remove the web reference by right-clicking it in the **Solution Explorer** and selecting **Delete** from the shortcut menu, then rebuild the project and repeat steps 14 through 21.
+   
+   29. Add the following variable declarations to the `ScriptMain` class of the component definition:
+   
+   ```csharp
+   private String _countryName;
+   private String _airportXPath = @"/NewDataSet/Table";
+   private XmlNodeList _airportXmlNodes;
+   ```
+   
+   30. Add the following commands to the `PreExecute()` method:
+   
+   ```csharp
+   airport airportInfo = new airport();
+   String airportInfoByCountry =
+   airportInfo.GetAirportInformationByCountry(_countryName);
+   XmlDocument airportInfoByCountryXml = new XmlDocument();
+   airportInfoByCountryXml.LoadXml(airportInfoByCountry);
+   _airportXmlNodes = airportInfoByCountryXml.SelectNodes(_airportXPath);
+   ```
+   
+   31. The `GetAirportInformationByCountry` function of the given web service returns a list of airports and their properties for the supplied country name. This function only needs to be invoked once per data flow execution; therefore, you should place it in the `PreExecute()` method, which is invoked once, at the beginning of the data flow execution.
