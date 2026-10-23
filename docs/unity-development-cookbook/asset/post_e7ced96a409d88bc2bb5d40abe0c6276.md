
```
Debug.LogWarning("Cannot fade - no canvas group attached!");
yield break;
}

// Fade time must be more than 0 in order for a fade to be
// animated
if (fadeTime <= 0) {
    yield break;
}

// Keep track of how much time we've spent fading
var fadeTimeElapsed = 0f;

// Perform the fade every frame
while (fadeTimeElapsed < fadeTime) {

    fadeTimeElapsed += Time.deltaTime;

    // Calculate the fraction of the fade time (between 0 and 1)
    var t = fadeTimeElapsed / fadeTime;

    // Calculate our alpha; it starts at 1, and goes to 0
    var alpha = 1f - t;

    // Apply the fade
    canvasGroup.alpha = alpha;

    // Wait for the next frame
    yield return null;
}

// Remove this game object from the scene
Destroy(gameObject);
}
```

- 11. Attach a FadeAfterDelay component to the Fading List Item.
- 12. Run the program. When you click the button, a new list item will appear; after a moment, it will fade out. We’d take a screenshot and show you, but it already faded away....

## Discussion

You’ll notice that the Start method is a coroutine, so other things can still happen while it runs and takes care of fading and deleting the object from memory.

## 12.6 Creating Onscreen Position Indicators

### Problem

You want to display icons that highlight the position of objects onscreen, providing the player with a general direction for objects that they might not be able to see at the time.

### Solution

To display the indicators, we’ll use Unity’s GUI system. This means that we’ll set up a canvas and a prototype indicator to use as a prefab.

You’ll need a sprite to use for your indicators. If you don’t have one, Kenney’s game icons pack has a good one. To create onscreen position indicators:

- 1. Create a new canvas by opening the GameObject menu and choosing UI → Canvas.
- 2. Create a new image by opening the GameObject menu and choosing UI → Image.
- 3. Set the sprite of the image to the sprite you want to use for your indicators.
- 4. Rename the image “Indicator.”
- 5. Drag the Indicator image into the Project tab. This will create a prefab.
- 6. Delete the Indicator image from the scene.

Next, we’ll create the code that creates, positions, and removes indicators as they’re needed:

- 1. Create a new C# script called IndicatorManager.cs, and add the following code to it:

```
public class IndicatorManager : MonoBehaviour {

    // The indicator that appears over each tracked object.
    [SerializeField] RectTransform indicatorPrefab = null;

    // The object that all indicators will go into.
    [SerializeField] RectTransform indicatorContainer = null;

    // The single instance of the indicator manager.
    public static IndicatorManager manager;

    // Maps objects in the world to indicators onscreen.
    Dictionary<TrackedObject, RectTransform> indicators =
        new Dictionary<TrackedObject, RectTransform>();
```

```
private void Awake()
{
    // Set up the singleton variable to refer to this instance.
    manager = this;
}

private void LateUpdate()
{
    // We do this in LateUpdate so that the calculation of the
    // positions can happen after the objects have moved, which
    // prevents jitter.

    // Every frame, for each object that we're tracking, update the
    // position of its indicator.
    foreach (var pair in indicators) {
        TrackedObject target = pair.Key;
        RectTransform indicator = pair.Value;

        // Has the target been removed from the scene?
        if (target == null) {
            // Skip this indicator
            continue;
        }

        // Update the indicator's position in the canvas.
        indicator.anchoredPosition =
            GetCanvasPositionForTarget(target);
    }
}

// Returns the location in canvas space that an indicator should be
// for a given object
private Vector2 GetCanvasPositionForTarget(TrackedObject target)
{
    // Convert the position of the object from world space to
    // viewport space
    var indicatorPoint =
        Camera.main.WorldToViewportPoint(target.transform.position);

    // Viewport coordinates are (0, 0) to (1, 1); (0, 0) is the
    // bottom-left corner of the screen.

    // If a point is outside the screen, we clamp it to the edges.
    indicatorPoint.x = Mathf.Clamp01(indicatorPoint.x);
    indicatorPoint.y = Mathf.Clamp01(indicatorPoint.y);

    // If a point is behind the camera, we force it to the bottom of
    // the screen.
    if (indicatorPoint.z < 0) {
```

```
indicatorPoint.y = 0;

// We also have to flip it on the x-axis for it to appear
// correctly.
indicatorPoint.x = 1f - indicatorPoint.x;
}

// Canvas coordinates are (0, 0) -> (width, height); (0, 0) is
// the bottom-left corner of the canvas.

// This means that we can scale by the canvas size to get the
// position in canvas space.

// Get the canvas
var canvas = indicatorContainer.GetComponentInParent<Canvas>();

// Get its size
Vector2 canvasSize =
    canvas.GetComponent<RectTransform>().sizeDelta;

// Scale it
indicatorPoint.Scale(canvasSize);

// We've now calculated where it belongs in the canvas!
return indicatorPoint;
}

public void AddTrackingIndicator(TrackedObject transform) {

    // Do we already have an indicator for this object?
    if (indicators.ContainsKey(transform)) {
        // Nothing to do; we already have an indicator for this
        // transform
        return;
    }

    // Create our indicator from the prefab
    var indicator = Instantiate(indicatorPrefab);

    // Give it a useful name
    indicator.name = string.Format("Indicator for {0}",
        transform.gameObject.name);

    // Move the indicator into the container
    indicator.SetParent(indicatorContainer, false);
```

```
// Ensure the pivot point is in the center of the object, so
// that the center of the image is right over the object's
// position
indicator.pivot = new Vector2(0.5f, 0.5f);

// Ensure the object doesn't adjust its size and position based
// on the size of its parent
indicator.anchorMin = Vector2.zero;
indicator.anchorMax = Vector2.zero;

// Keep track of the relationship between the target and its
// indicator
indicators[transform] = indicator;

// Place the indicator in the right location
indicator.anchoredPosition =
    GetCanvasPositionForTarget(transform);
}

// Stops tracking a target.
public void RemoveTrackingIndicator(TrackedObject transform) {
    // If we have an indicator for this target object, remove it
    // from the scene
    if (indicators.ContainsKey(transform)) {
        // Destroy the indicator, if it isn't already gone from the
        // scene.
        if (indicators[transform] != null) {
            Destroy(indicators[transform].gameObject);
        }
    }

    // And remove it from the list, if it's present. (The Remove
    // method won't throw an exception if "transform" isn't in the
    // dictionary.)
    indicators.Remove(transform);
}
```

- 2. Create an empty game object. Name it “Indicator Manager.”
- 3. Add an IndicatorManager component to the Indicator Manager.
- 4. Drag the canvas into the Indicator Container field.
- 5. Drag the Indicator prefab into the Indicator Prefab field.

Next, we’ll create a script that requests an indicator when it first appears, and removes it when it’s removed from the scene. Create a new C# script called TrackedObject.cs, and add the following code to it:

```
public class TrackedObject : MonoBehaviour {

    void Start() {
        // When the object first appears, request an indicator.
        IndicatorManager.manager.AddTrackingIndicator(this);
    }

    // Tell the indicator manager to remove our tracking indicator.
    // OnDestroy is called when either the object is removed from the scene,
    // or the scene is being unloaded (including when we exit Play mode).
    private void OnDestroy()
    {
        IndicatorManager.manager.RemoveTrackingIndicator(this);
    }

}
```

Finally, we’ll create an object that will have an indicator drawn over it:

- 1. Create a new sphere by opening the GameObject menu and choosing 3D Object → Sphere.
- 2. Move the sphere where the camera can see it.
- 3. Add a TrackedObject component to it.
- 4. Run the game. An indicator will be drawn over it; when you move the sphere around, the indicator will follow it. If you move the sphere behind the camera, its indicator will move to the bottom of the screen; when you delete the sphere, the indicator will be removed as well. You can also add as many TrackedObject components to the scene as you’d like.

## Discussion

Don’t forget that objects are destroyed when the Unity editor leaves Play mode!

## 12.7 Custom Editors

### Problem

You want to customize the Inspector for a component.
