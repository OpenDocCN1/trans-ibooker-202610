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