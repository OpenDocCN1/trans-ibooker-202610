Additionally, we'll make this subclass abstract, so that we're implementing only the SaveID property, and leaving the SaveData and LoadFromData methods up to the subclasses.

Create a new C# script called SaveableBehaviour.cs, and add the following code to it:

```
public abstract class SaveableBehaviour : MonoBehaviour,
    ISaveable,
    ISerializationCallbackReceiver // Asks Unity to run code when the
    // scene file is saved in the editor
{
    // This class doesn't implement SavedData or LoadFromData; that's the
    // job of our subclasses.
    public abstract JsonData SavedData { get; }
    public abstract void LoadFromData(JsonData data);

    // This class does implement the SaveID property; it wraps the _saveID
    // field. (We need to do this manually, rather than using automatic
    // property generation (i.e., "public string SaveID {get;set;}"),
    // because Unity won't store automatic properties when saving the
    // scene file.

    public string SaveID
    {
        get
        {
            return _saveID;
        }
        set
        {
            _saveID = value;
        }
    }

    // The _saveID field stores the actual data that SaveID uses. We mark
    // it as serialized so that the Unity editor saves it with the rest of the
    // scene, and ask HideInInspector to make it not appear in the Inspector
    // (there's no reason for it to be edited).
    [HideInInspector]
    [SerializeField]
    private string _saveID;

    // OnBeforeSerialize is called when Unity is about to save this object
    // as part of a scene file.
```