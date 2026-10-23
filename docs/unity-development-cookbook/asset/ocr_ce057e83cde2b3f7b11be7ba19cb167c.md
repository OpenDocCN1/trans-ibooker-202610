of the scene; rather, it allows objects to choose what data they need saved, and what to do with data that’s been loaded.

We’ll do all of our saving and loading using JSON, for two reasons:

+   - JSON is a text format, which makes it a bit easier to look at the generated result and diagnose problems.
+   - JSON doesn’t rely on a fixed schema, which means it takes a little less work to get things working.

The overall strategy for this saving and loading system is as follows:

+   - We’ll define a C# interface called ISaveable; the MonoBehaviours that can save their state implement this interface.
+   - Saving means finding all MonoBehaviours that implement this interface, asking for their save data, and using that data to create the save file.
+   - Loading means loading the save file, looking up every MonoBehaviour that implements ISaveable using its ID, and giving that object the information to use to restore it.

The specifics of what each ISaveable MonoBehaviour provides when it’s asked to provide its save state, and what it does when it receives the saved state, are up to it.

To understand how the saving system works, it’s helpful to see what a save file looks like. Here’s an example of one; we’ve added comments to explain each part:

```
{
    // "objects" is a list of objects that stores the data of each saved
    // ISaveable. This saved game contains only a single saved object,
    // but you can have as many as you like.
    "objects" : [
    {
        // Each object has a unique Save ID, which is used to find
        // the object later when the game is loaded.
        "$saveID": "805133b3-fad9-41ed-a253-fce07e3c4672",

        // The data inside each saved object is determined by the
        // object itself.
        "localPosition": {
            "x" : -1.97000002861023,
            "y" : 0.0,
            "z" : 0.0
        },
        "localRotation": {
            "x" : 0.0,
            "y" : 0.0,
```