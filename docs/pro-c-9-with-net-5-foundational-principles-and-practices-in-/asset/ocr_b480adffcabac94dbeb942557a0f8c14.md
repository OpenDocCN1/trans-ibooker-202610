```
new JamesBondCar { CanFly = true, CanSubmerge = true },
new JamesBondCar { CanFly = true, CanSubmerge = false },
new JamesBondCar { CanFly = false, CanSubmerge = true },
new JamesBondCar { CanFly = false, CanSubmerge = false },
};

File.WriteAllText(fileName, System.Text.Json.JsonSerializer.Serialize(myCars, options));
Console.WriteLine("=> Saved list of cars!");
}
```

To wrap it all up, add the following line to exercise the new function:

```
SaveListOfCarsAsJson(options, "CarCollection.json");
```

# Deserializing Objects and Collections of Objects

Just like XML deserialization, JSON deserialization is the opposite of serialization. The following function will deserialize JSON in the type specified using the generic version of the method:

```
static T ReadAsJsonFormat<T>(JsonSerializerOptions options, string fileName) =>
    System.Text.Json.JsonSerializer.Deserialize<T>(File.ReadAllText(fileName), options);
```

Add the following code to the top-level statements to reconstitute your XML back into objects (or list of objects):

```
JamesBondCar savedJsonCar = ReadAsJsonFormat<JamesBondCar>(options, "CarData.json");
Console.WriteLine("Read Car: {0}", savedJsonCar.ToString());

List<JamesBondCar> savedJsonCars = ReadAsJsonFormat<List<JamesBondCar>>(options,
"CarCollection.json");
Console.WriteLine("Read Car: {0}", savedJsonCar.ToString());
```

# Summary

You began this chapter by examining the use of the Directory(Info) and File(Info) types. As you learned, these classes allow you to manipulate a physical file or directory on your hard drive. Next, you examined several classes derived from the abstract Stream class. Given that Stream-derived types operate on a raw stream of bytes, the System.IO namespace provides numerous reader/writer types (e.g., StreamWriter, StringWriter, and BinaryWriter) that simplify the process. Along the way, you also checked out the functionality provided by DriveType, learned how to monitor files using the FileSystemWatcher type, and saw how to interact with streams in an asynchronous manner.

This chapter also introduced you to the topic of object serialization services. As you have seen, the .NET Core platform uses an object graph to account for the full set of related objects that you want to persist to a stream. You then worked with XML and JSON serialization and deserialization.