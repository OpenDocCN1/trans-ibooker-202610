```
selectionBox.gameObject.SetActive(false);

// We're no longer selecting
IsSelecting = false;
}
}
```

```
// Called when the user finishes dragging a selection box.
void SelectionComplete()
{
```

```
// Get the component attached to this scene
Camera mainCamera = GetComponent<Camera>();
```

```
// Get the bottom-left and top-right corners of the screen-space
// selection view, and convert them to viewport space
var min = mainCamera.ScreenToViewportPoint(
    new Vector3(SelectionRect.xMin, SelectionRect.yMin));
var max = mainCamera.ScreenToViewportPoint(
    new Vector3(SelectionRect.xMax, SelectionRect.yMax));
```

```
// We want to create a bounding box in viewport space. We have
// the x and y coordinates of the bottom-left and top-right
// corners; now we'll include the z coordinates.
min.z = mainCamera.nearClipPlane;
max.z = mainCamera.farClipPlane;
```

```
// Construct our bounding box
var viewportBounds = new Bounds();
viewportBounds.SetMinMax(min, max);
```

```
// Check each object that has a Selectable component
foreach (var selectable in FindObjectsOfType<BoxSelectable>()) {
```