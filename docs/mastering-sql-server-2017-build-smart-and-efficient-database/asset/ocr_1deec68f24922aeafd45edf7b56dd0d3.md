```
}
}
```

This class allows you to store the pipeline data during processing, and it provides the metadata to the JavaScriptSerializer.Serialize() method that creates the JSON documents.

Unless explicitly marked **public**, class members are, by default, marked **private**, and thus *inaccessible* to the caller. If the metadata for the JSON document cannot be determined, the serialization will *not* fail, but the resulting document will also not contain any data for which the metadata was not available during serialization.

![](bbox=[0.12, 0.25, 0.19, 0.33])

+   22. Before the AirportInfo class definition, place the definition of the function that will be used to write the data to the file:

```
private void WriteJSONFile(List<AirportInfo> airportInfo,
Int32 fileCount)
{
    String airportInfoJSON = _jsonSerializer.Serialize(airportInfo);

    String jsonFileName = Path.Combine(_jsonFilePath,
    String.Format(_jsonFileNameFormat, fileCount));

    using (StreamWriter jsonStreamWriter = new
StreamWriter(jsonFileName))
    {
        jsonStreamWriter.Write(airportInfoJSON);
        jsonStreamWriter.Flush();
    }
}
```

+   23. Add the following variable assignments to the PreExecute() method:

```
_jsonFilePath = Variables.JSONFilePath;
_jsonFileNameFormat = Variables.JSONFileNameFormat;
```

+   24. Remove the JSONFileInput_ProcessInputRows method, as this component will not process individual rows in the traditional way.