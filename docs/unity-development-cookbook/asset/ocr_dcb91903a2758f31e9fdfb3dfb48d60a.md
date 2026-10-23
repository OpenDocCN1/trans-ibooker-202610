```
var searchPointWorldSpace =
    transform.TransformPoint(searchPointLocalSpace);

// Find the nearest point on the navmesh
NavMeshHit hit;

bool foundPoint;

foundPoint = NavMesh.SamplePosition(
    searchPointWorldSpace,
    out hit,
    5,
    NavMesh.AllAreas
);

if (foundPoint == false) {
    // We can't get here. Disregard as a place to hide.
    continue;
}

searchPointWorldSpace = hit.position;

var canSee =
    vistbility.CheckVisibilityToPoint(searchPointWorldSpace);

if (canSee == false) {
    // We can't see the target from this position.
    // Return it!
    candidateHidingSpots.Add(searchPointWorldSpace);
}

if (visualize) {
    Color debugColor = canSee ? Color.red : Color.green;

    Debug.DrawLine(
        transform.position, searchPointWorldSpace,
        debugColor, 0.1f);
}

}

if (candidateHidingSpots.Count == 0) {
    // We didn't find a hiding spot.

    // Provide a dummy value
    hidingSpot = Vector3.zero;

    // Indicate our failure
    return false;
```