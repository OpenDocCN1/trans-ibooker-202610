```
```csharp
if (withinArc == false) {
    return false;
}
```
```

```
```csharp
// Compute the distance to the point
var distanceToTarget = directionToTarget.magnitude;
```
```

```
```csharp
// Our ray should go as far as the target is or the maximum
// distance, whichever is shorter
var rayDistance = Mathf.Min(maxDistance, distanceToTarget);
```
```

```
```csharp
// Create a ray that fires out from our position to the target
var ray = new Ray(transform.position, directionToTarget);
```
```

```
```csharp
// Store information about what was hit in this variable
RaycastHit hit;
```
```

```
```csharp
// Records info about whether the target is in range and not
// occluded
var canSee = false;
```
```

```
```csharp
// Fire the raycast. Did it hit anything?
if (Physics.Raycast(ray, out hit, rayDistance))
{
    // Did the ray hit our target?
    if (hit.collider.transform == target)
    {
        // Then we can see it (that is, the ray didn't hit an
        // obstacle between us and the target)
        canSee = true;
    }
}
```
```

```
```csharp
// Visualize the ray.
Debug.DrawLine(transform.position, hit.point);
```
```

```
```csharp
}
else
{
    // The ray didn't hit anything. This means that it reached
```
```