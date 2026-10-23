```
{
    Debug.LogErrorFormat(
        "Data at {0} specifies an active scene that " +
        "doesn't exist. Stopping loading here.",
        dataPath
    );
    return false;
}

SceneManager.SetActiveScene(activeScene);
} else {
    // This is not an error, since the first scene in the list will
    // be treated as active, but it's worth warning about.
    Debug.LogWarningFormat("Data at {0} does not specify an " +
        "active scene.", dataPath);
}

// Find all objects in the scene and load them
if (data.ContainsKey(OBJECTS_KEY)) {
    var objects = data[OBJECTS_KEY];

    // We can't update the state of the objects right away because
    // Unity will not complete loading the scene until sometime in
    // the future. Changes we made to the objects would revert
    // to how they're defined in the original scene. As a result, we
    // need to run the code after the scene manager reports that a
    // scene has finished loading.

    // To do this, we create a new delegate that contains our object-
    // loading code, and store that in LoadObjectsAfterSceneLoad.
    // This delegate is added to the SceneManager's sceneLoaded
    // event, which makes it run after the scene has finished loading.

    LoadObjectsAfterSceneLoad = (scene, loadSceneMode) => {

        // Find all ISaveable objects, and build a dictionary that maps
        // their Save IDs to the object (so that we can quickly look
        // them up)
        var allLoadableObjects = Object
            .FindObjectsOfType<MonoBehaviour>()
            .OfType<ISaveable>()
            .ToDictionary(o => o.SaveID, o => o);

        // Get the collection of objects we need to load
        var objectsCount = objects.Count;

        // For each item in the list...
        for (int i = 0; i < objectsCount; i++)
        {
            // Get the saved data
            var objectData = objects[i];
```