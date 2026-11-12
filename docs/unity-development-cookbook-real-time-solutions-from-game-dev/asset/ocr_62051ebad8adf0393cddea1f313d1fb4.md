```
// Maps objects in the world to indicators onscreen.
Dictionary<TrackedObject, RectTransform> indicators =
    new Dictionary<TrackedObject, RectTransform>();

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
```