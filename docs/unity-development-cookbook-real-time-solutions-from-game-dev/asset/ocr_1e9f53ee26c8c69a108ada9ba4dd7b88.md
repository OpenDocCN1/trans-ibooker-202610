```
// Is the user pressing the jump button right now?
if (Input.GetButton("Jump"))
{
    // Calculate the amount of upward speed we need,
    // considering that we add moveDirection.y to our height
    // every frame, and we reduce moveDirection.y by gravity
    // every frame.
    moveDirection.y = Mathf.Sqrt(2 * gravity * jumpHeight);
} else {
    // We're on the ground, but not jumping. Set our
    // downward movement to 0 (otherwise, because we're
    // continuously reducing our y-movement, if we walk off
    // a ledge, we'd suddenly have a huge amount of
    // downward momentum).
    moveDirection.y = 0;
}
} else {
    // Slowly bring our movement toward the user's desired
    // input, but preserve our current y-direction (so that the
    // arc of the jump is preserved)
    input.y = moveDirection.y;
    moveDirection = Vector3.Lerp(moveDirection, input,
        airControl * Time.deltaTime);
}

// Bring our movement down by applying gravity over time
moveDirection.y -= gravity * Time.deltaTime;

// Move the controller. The controller will refuse to move into
// other colliders, which means that we won't clip through the
// ground or other colliders. (However, this doesn't stop other
// colliders from moving into us. For that, we'd need to detect
// when we're overlapping another collider, and move away from
```