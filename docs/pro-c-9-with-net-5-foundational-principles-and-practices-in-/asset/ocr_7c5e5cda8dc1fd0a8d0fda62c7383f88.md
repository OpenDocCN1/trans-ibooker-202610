# Table 20-13. Select Attributes of the System.Text.Json.Serialization Namespace

| .NET Attribute | Meaning in Life |
|---|---|
| [JsonIgnore] | The property will be ignored. |
| [JsonInclude] | The member will be included. |
| [JsonPropertyName] | Specifies the property name to be used when serializing/deserializing a member. This is commonly used to resolve character casing issues. |
| [JsonConstructor] | Indicates the constructor that should be used when deserializing JSON back into an object graph. |

# Serializing Objects Using the JsonSerializer

The JsonSerializer contains static Serialize methods used to convert .NET Core objects (including object graphs) into a string representation of the public properties. The data is represented as name-value pairs in JavaScript Object Notation. Consider the following local function added to your Program.cs class:

```
static void SaveAsJsonFormat<T>(T objGraph, string fileName)
{
    File.WriteAllText(fileName, System.Text.Json.JsonSerializer.Serialize(objGraph));
}
```

Add the following code to your top-level statements:

```
SaveAsJsonFormat(jbc, "CarData.json");
Console.WriteLine("=> Saved car in JSON format!");

SaveAsJsonFormat(p, "PersonData.json");
Console.WriteLine("=> Saved person in JSON format!");
```

When you examine the created JSON files, you might be surprised to see that the CarData.json file is empty (except for a pair of braces) and the PersonData.json file contains only the Firstname value. This is because the JsonSerializer only writes public properties by default, and not public fields. You will correct this in the next section.

# Including Fields

To include public fields into the generated JSON, you have two options. The other method is to use the JsonSerializerOptions class to instruct the JsonSerializer to include all fields. The second is to update your classes by adding the [JsonInclude] attribute to each public field that should be included in the JSON output. Note that the first method (using the JsonSerializationOptions) will include all public fields in the object graph. To exclude certain public fields using this technique, you must use the JsonExclude attribute on those to be excluded.

Update the SaveAsJsonFormat method to the following:

```
static void SaveAsJsonFormat<T>(T objGraph, string fileName)
{
    var options = new JsonSerializerOptions
    {
        IncludeFields = true,
    };
    File.WriteAllText(fileName, System.Text.Json.JsonSerializer.Serialize(objGraph, options));
}
```