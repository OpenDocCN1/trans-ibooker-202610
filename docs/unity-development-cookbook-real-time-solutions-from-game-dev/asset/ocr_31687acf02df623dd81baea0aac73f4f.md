```
{
    transform.localPosition =
    DeserializeValue<Vector3>(data[LOCAL_POSITION_KEY]);
}

// Update rotation
if (data.ContainsKey(LOCAL_ROTATION_KEY))
{
    transform.localRotation =
    DeserializeValue<Quaternion>(data[LOCAL_ROTATION_KEY]);
}

// Update scale
if (data.ContainsKey(LOCAL_SCALE_KEY))
{
    transform.localScale =
    DeserializeValue<Vector3>(data[LOCAL_SCALE_KEY]);
}
}
```

2. To use this script, add a TransformSaver component to your game object. It will automatically be included in saved games, and its state will be restored when the game is loaded.

# Discussion

This system doesn't automatically restore any objects that were created after the scene loaded. If you want to support this, you'll need to have an object in your scene that keeps references to objects that it creates, and stores the additional information about those objects when it's asked for its save data. When the object is asked to load, it re-creates the objects.