```
// Called by Unity to figure out the height of this property.
public override float GetPropertyHeight(SerializedProperty property, GUIContent label)
{
    // The number of lines in this property
    var lineCount = 3;

    // The number of pixels between each line
    var lineSpacing = 2;

    // The height of each line
    var lineHeight = base.GetPropertyHeight(property, label);

    // The height of this property is the number of lines times the
    // height of each line, plus the spacing between each line
    return (lineHeight * lineCount) +
           (lineSpacing * (lineCount - 1));
}
#endif
```

Finally, we’ll set up the cubes so that they complete or fail objectives when they’re clicked:

+   1. Create a new C# script called TriggerObjectiveOnClick.cs, and add the following code to it:

```
using UnityEngine.EventSystems;

// Triggers an objective when an object enters it.
public class TriggerObjectiveOnClick :
    MonoBehaviour, IPointerClickHandler {

    // The objective to trigger, and how to trigger it.
    [SerializeField] ObjectiveTrigger objective = new ObjectiveTrigger();

    // Called when the player clicks on this object
    void IPointerClickHandler.OnPointerClick(PointerEventData eventData)
    {
        // We just completed or failed this objective!
        objective.Invoke();

        // Disable this component so that it doesn’t get run twice
        this.enabled = false;
    }
}
```