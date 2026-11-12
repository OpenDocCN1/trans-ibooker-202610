```
{
    targetIsVisible = CheckVisibility();

    if (visualize) {
        // Update our color: yellow if we can see the target,
        // white if we can't
        var color = targetIsVisible ? Color.yellow : Color.white;

        GetComponent<Renderer>().material.color = color;
    }
}
```

```
// Returns true if this object can see the specified position.
public bool CheckVisibilityToPoint(Vector3 worldPoint) {
```

```
// Calculate the direction from our location to the point
var directionToTarget = worldPoint - transform.position;
```

```
// Calculate the number of degrees from the forward direction.
var degreesToTarget =
    Vector3.Angle(transform.forward, directionToTarget);
```

```
// The target is within the arc if it's within half of the
// specified angle. If it's not within the arc, it's not visible.
var withinArc = degreesToTarget < (angle / 2);
```

```
if (withinArc == false)
{
    return false;
}
```

```
// Figure out the distance to the target
var distanceToTarget = directionToTarget.magnitude;
```

```
// Take into account our maximum distance
var rayDistance = Mathf.Min(maxDistance, distanceToTarget);
```