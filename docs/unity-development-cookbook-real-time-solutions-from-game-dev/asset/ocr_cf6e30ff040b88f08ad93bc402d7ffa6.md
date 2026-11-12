To demonstrate how to build a script that can save its data, we'll build an example, TransformSaver.cs, that stores information about the position, rotation, and scale of the game object that it's attached to:

+   1. Create a new C# script called TransformSaver.cs, and add the following code:

```
public class TransformSaver : SaveableBehaviour
{
    // Store the keys we'll be including in the saved game as constants,
    // to avoid problems with typos.
    private const string LOCAL_POSITION_KEY = "localPosition";
    private const string LOCAL_ROTATION_KEY = "localRotation";
    private const string LOCAL_SCALE_KEY = "localScale";

    // SerializeValue is a helper function that converts an object that
    // Unity already knows how to serialize (like Vector3, Quaternion,
    // and others) into a JsonData that can be included in the
    // saved game.
    private JsonData SerializeValue(object obj) {
        // This is very inefficient (we're converting an object to JSON
        // text, then immediately parsing this text back into a JSON
        // representation), but it means that we don't need to write the
        // (de)serialization code for built-in Unity types
        return JsonMapper.ToObject(JsonUtility.ToJson(obj));
    }

    // DeserializeValue works in reverse: given a JsonData, it produces
    // a value of the desired type, as long as that type is one that
    // Unity already knows how to serialize.
    private T DeserializeValue<T>(JsonData data) {
        return JsonUtility.FromJson<T>(data.ToJson());
    }
}
```