```
// Get the Save ID from that data
var saveID = (string)objectData[SAVEID_KEY];

// Attempt to find the object in the scene(s) that has that
// Save ID
if (allLoadableObjects.ContainsKey(saveID))
{
    var loadableObject = allLoadableObjects[saveID];

    // Ask the object to load from this data
    loadableObject.LoadFromData(objectData);
}

// Tidy up after ourselves; remove this delegate from the
// sceneLoaded event so that it isn't called next time
SceneManager.sceneLoaded -= LoadObjectsAfterSceneLoad;

// Release the reference to the delegate
LoadObjectsAfterSceneLoad = null;

// And ask the garbage collector to tidy up (again, this will
// cause a performance hitch, but users are fine with this as
// they're already waiting for the scene to finish loading)
System.GC.Collect();
};

// Register the object-loading code to run after the scene loads.
SceneManager.sceneLoaded += LoadObjectsAfterSceneLoad;

return true;
}
```

The LoadGame method reads the file and first loads the scenes that are specified in the saved game. After that, it creates and saves the code that restores the state of the objects in the scenes, and adds it to the SceneManager's sceneLoaded event. The code itself finds all ISaveable MonoBehaviours, creates a dictionary that maps their Save ID to the object itself, and then uses the loaded object data to both find the appropriate object for the data and deliver the data to that object.

To save the game, you call the SaveGame method on the SavingService class:

```
// Save the game to a file called "SaveGame.json"
SavingService.SaveGame("SaveGame.json");
```

To load the game, you call the LoadGame method:

```
// Try to load the game from a file called "SaveGame.json"
SavingService.LoadGame("SaveGame.json");
```