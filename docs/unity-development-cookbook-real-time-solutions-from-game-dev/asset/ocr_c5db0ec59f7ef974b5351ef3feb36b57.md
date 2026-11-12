```
// write it to disk.

// Figure out where to put the file by combining the persistent
// data path with the filename that this method received as a
// parameter.
var outputPath = Path.Combine(
    Application.persistentDataPath, fileName);

// Create a JsonWriter, and configure it to 'pretty-print' the
// data. This is optional (you could just call result.ToJson() with
// no JsonWriter parameter and receive a string), but this way the
// resulting JSON is easier to read and understand, which is
// helpful while developing.
var writer = new JsonWriter();
writer.PrettyPrint = true;

// Convert the save data to JSON text.
result.ToJson(writer);

// Write the JSON text to disk.
File.WriteAllText(outputPath, writer.ToString());

// Notify where to find the saved game
Debug.LogFormat("Wrote saved game to {0}", outputPath);

// We allocated a lot of memory here, which means that there's an
// increased chance of the garbage collector needing to run in the
// future. To tidy up, we'll release our reference to the saved
// data, and then ask the garbage collector to run immediately.
// This will result in a slight performance hitch as the collector
// runs, but that's fine for this case, since users expect saving
// the game to pause for a second.
result = null;
```