```
void FixedUpdate () {
    // Move toward the target at a fixed speed
    var newPosition = Vector3.MoveTowards(
    transform.position, currentPoint, speed * Time.deltaTime);

    // Have we reached the target?
    if (Vector3.Distance(newPosition, currentPoint) < 0.001) {
    // Snap to the target point
    newPosition = currentPoint;

    // Move to the next target, wrapping around to the start if
    // necessary
    nextPoint += 1;
    nextPoint %= points.Length;
    }

    // Calculate our current velocity in units per second
    velocity =
    (newPosition - transform.position) / Time.deltaTime;

    // Update to our new location
    transform.position = newPosition;
}

// Draw the path that the platform will follow
private void OnDrawGizmosSelected()
{
    if (points == null || points.Length < 2) {
    return;
    }

    // Our points are stored in local space, so we need to offset
```