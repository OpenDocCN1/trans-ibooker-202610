4. We can now create the SaveGame method, which performs the work of actually saving the scene. Add the following method to the SavingService class:

```
// Saves the game, and writes it to a file called fileName in the app's
// persistent data directory.
public static void SaveGame(string fileName) {

    // Create the JsonData that we will eventually write to disk
    var result = new JsonData();

    // Find all MonoBehaviours by first finding every MonoBehaviour,
    // and filtering it to only include those that are ISaveable.
    var allSaveableObjects = Object
        .FindObjectsOfType<MonoBehaviour>()
        .OfType<ISaveable>();

    // Do we have any objects to save?
    if (allSaveableObjects.Count() > 0) {

        // Create the JsonData that will store the list of objects
        var savedObjects = new JsonData();

        // Iterate over every object we want to save
        foreach (var saveableObject in allSaveableObjects)
        {
            // Get the object's saved data
            var data = saveableObject.SavedData;

            // We expect this to be an object (JSON's term for a
            // dictionary) because we need to include the object's Save
            // ID
            if (data.IsObject) {

                // Record the Save ID for this object
                data[SAVEID_KEY] = saveableObject.SaveID;

                // Add the object's save data to the collection
                savedObjects.Add(data);
            } else {

                // Provide a helpful warning that we can't save this
                // object.
                var behaviour = saveableObject as MonoBehaviour;

                Debug.LogWarningFormat(
                    behaviour,
                    "{0}'s save data is not a dictionary. The " +
                    "object was not saved.",
                    behaviour.name
                );
            }
        }
    }
```