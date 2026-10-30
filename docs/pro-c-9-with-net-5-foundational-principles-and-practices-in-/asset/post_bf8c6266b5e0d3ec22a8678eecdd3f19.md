
If you want to specify a custom XML namespace that qualifies the JamesBondCar and encodes the canFly and canSubmerge values as XML attributes, you can do so by modifying the C# definition of JamesBondCar, like so:

```
[Serializable, XmlRoot(Namespace = "http://www.MyCompany.com")]
public class JamesBondCar : Car
{
    [XmlAttribute]
    public bool CanFly;
    [XmlAttribute]
    public bool CanSubmerge;
    ...
}
```

This yields the following XML document (note the opening <JamesBondCar> element):

```
<?xml version="1.0"?>
<JamesBondCar xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    CanFly="true" CanSubmerge="false" xmlns="http://www.MyCompany.com">
...
</JamesBondCar>
```

Next, examine the following PersonData.xml file:

```
<?xml version="1.0"?>
<Person xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <IsAlive>true</IsAlive>
    <FirstName>James</FirstName>
</Person>
```

Notice how the PersonAge property is not serialized into the XML. This confirms that XML serialization only serializes public properties and fields.

## Serializing Collections of Objects

Now that you have seen how to persist a single object to a stream, you are ready to examine how to save a set of objects. Create a local function that initializes a list of JamesBondCars and then serializes them to XML.

```
static void SaveListOfCarsAsXml()
{
    //Now persist a List<T> of JamesBondCars.
    List<JamesBondCar> myCars = new()
    {
        new JamesBondCar{CanFly = true, CanSubmerge = true},
        new JamesBondCar{CanFly = true, CanSubmerge = false},
        new JamesBondCar{CanFly = false, CanSubmerge = true},
        new JamesBondCar{CanFly = false, CanSubmerge = false},
    };

```

```
using (Stream fStream = new FileStream("CarCollection.xml",
    FileMode.Create, FileAccess.Write, FileShare.None))
{
    XmlSerializer xmlFormat = new XmlSerializer(typeof(List<JamesBondCar>));
    xmlFormat.Serialize(fStream, myCars);
}
Console.WriteLine("=> Saved list of cars!");
}
```

To wrap it all up, add the following line to exercise the new function:

```
SaveListOfCarsAsXml();
```

## Deserializing Objects and Collections of Objects

XML deserialization is literally the opposite of serializing objects (and collections of objects). Consider the following local function to deserialize XML back into an object graph. Notice that, once again, the type to work with must be passed into the constructor for the XmlSerializer:

```
static T ReadAsXmlFormat<T>(string fileName)
{
    // Create a typed instance of the XmlSerializer
    XmlSerializer xmlFormat = new XmlSerializer(typeof(T));
    using (Stream fStream = new FileStream(fileName, FileMode.Open))
    {
        T obj = default;
        obj = (T)xmlFormat.Deserialize(fStream);
        return obj;
    }
}
```

Add the following code to the top-level statements to reconstitute your XML back into objects (or list of objects):

```
JamesBondCar savedCar = ReadAsXmlFormat<JamesBondCar>("CarData.xml");
Console.WriteLine("Original Car: {0}",savedCar.ToString());
Console.WriteLine("Read Car: {0}",savedCar.ToString());

List<JamesBondCar> savedCars = ReadAsXmlFormat<List<JamesBondCar>>("CarCollection.xml");
```

## Serializing and Deserialization with System.Text.Json

The System.Text.Json namespace provides the System.Text.Json.JsonSerializer. You can use this formatter to persist the public state of a given object as JSON.

## Controlling the Generated JSON Data

By default, JsonSerializer serializes all public properties as JSON name-value pairs using the same name (and casing) of the object's property names. You can control many aspects of the serialization process with the most used attributes listed in Table 20-13.

## Table 20-13. Select Attributes of the System.Text.Json.Serialization Namespace

| .NET Attribute | Meaning in Life |
|---|---|
| [JsonIgnore] | The property will be ignored. |
| [JsonInclude] | The member will be included. |
| [JsonPropertyName] | Specifies the property name to be used when serializing/deserializing a member. This is commonly used to resolve character casing issues. |
| [JsonConstructor] | Indicates the constructor that should be used when deserializing JSON back into an object graph. |

## Serializing Objects Using the JsonSerializer

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

## Including Fields

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

Instead of using the JsonSerializerOptions, you can achieve the same result by updating all public fields in the sample classes to the following (note that you can leave the Xml attributes in the classes and they will not interfere with the JsonSerializer):

```
//Radio.cs
public class Radio
{
    [JsonInclude]
    public bool HasTweeters;
    [JsonInclude]
    public bool HasSubWoofers;
    [JsonInclude]
    public List<double> StationPresets;
    [JsonInclude]
    public string RadioId = "XF-552RR6";
    ...
}

//Car.cs
public class Car
{
    [JsonInclude]
    public Radio TheRadio = new Radio();
    [JsonInclude]
    public bool IsHatchBack;
    ...
}

//JamesBondCar.cs
public class JamesBondCar : Car
{
    [XmlAttribute]
    [JsonInclude]
    public bool CanFly;
    [XmlAttribute]
    [JsonInclude]
    public bool CanSubmerge;
    ...
}

//Person.cs
public class Person
{
    // A public field.
    [JsonInclude]
    public bool IsAlive = true;
    ...
}
```

Now when you run the code using either method, all public properties and fields are written to the file. However, when you examine the contents, you will see that the JSON is written minified. Minified is a format where all insignificant white space and line breaks are removed. This is the default format largely due

to JSON’s wide use for RESTful services and reduces the size of the data packet when sending information between services over HTTP/HTTPS.

> Note The field handling for serializing JSON is the same as deserializing JSON. If you chose to set the option to include fields when serializing JSON, you must also include that option when deserializing JSON.

## Pretty-Print JSON

In addition to the option to include public fields, The JsonSerializer can be instructed to write the JSON indented (and human readable). Update your method to the following:

```
static void SaveAsJsonFormat<T>(T objGraph, string fileName)
{
    var options = new JsonSerializerOptions
    {
        IncludeFields = true,
        WriteIndented = true
    };
    File.WriteAllText(fileName, System.Text.Json.JsonSerializer.Serialize(objGraph, options));
}
```

Now examine the CarData.json file, and the output is much more readable.

```
{
  "CanFly": true,
  "CanSubmerge": false,
  "TheRadio": {
    "HasTweeters": true,
    "HasSubWoofers": false,
    "StationPresets": [
      89.3,
      105.1,
      97.1
    ],
    "RadioId": "XF-552RR6"
  },
  "IsHatchBack": false
}
```

## PascalCase or camelCase JSON

Pascal casing is a format that uses the first character capitalized and every significant part of a name capitalized as well. Take the previous JSON listing. CanSubmerge is an example of Pascal casing. Camel casing, on the other hand, sets the first character to lowercase (like the word camelCase in the title of this section), and then every significant part of the name starts with a capital. The camel case version of the previous example is canSubmerge.

Why does this matter? It matters because most of the popular languages are case sensitive (like C#). That means the CanSubmerge and canSubmerge are two different items. As you have seen throughout this book, the generally accepted standard for naming public things in C# (classes, public properties, functions,
