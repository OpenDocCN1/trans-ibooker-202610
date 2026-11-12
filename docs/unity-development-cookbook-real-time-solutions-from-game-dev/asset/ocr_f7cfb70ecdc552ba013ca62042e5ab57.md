```
// Create a new ray that goes from our current location, in the
// specified direction
var ray = new Ray(transform.position, directionToTarget);
```

```
// Stores information about anything we hit
RaycastHit hit;
```

```
// Perform the raycast. Did it hit anything?
if (Physics.Raycast(ray, out hit, rayDistance)) {
    // We hit something.
    if (hit.collider.transform == target) {
        // It was the target itself. We can see the target point.
        return true;
    }
    // It's something between us and the target. We cannot see
    // the target point.
    return false;
} else {
    // There's an unobstructed line of sight between us and the
    // target point, so we can see it.
    return true;
}
```

```
// Returns true if a straight line can be drawn between this object
// and the target. The target must be within range, and within the
// visible arc.
public bool CheckVisibility()
{
```

```
// Compute the direction to the target
var directionToTarget = target.position - transform.position;
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