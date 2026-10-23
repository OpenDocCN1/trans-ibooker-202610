```
[SerializeField] float pushPower = 5;

// Called when a character collider on the object that this script
// is attached to touches any other collider.
private void OnControllerColliderHit(ControllerColliderHit hit)
{
    // Immediately exit if pushing is disabled
    if (pushMode == PushMode.NoPushing)
    {
        return;
    }

    // Get the rigidbody attached to the collider we hit
    var hitRigidBody = hit.rigidbody;

    // Is this rigidbody something we can push?
    if (hitRigidBody == null || hitRigidBody.isKinematic == true)
    {
        // Either it doesn't have a rigidbody, or the rigidbody
        // is kinematic (that is, it doesn't respond to
        // external forces).

        // Since we're going to apply a force to it, we should
        // respect its settings.
        return;
    }

    // Get a reference to the controller that hit the object, since
    // we'll be making references to it often.
    CharacterController controller = hit.controller;

    // Calculate the world position of the lowest point on the
    // controller.
    var footPosition = controller.transform.position.y
        - controller.center.y
        - controller.height / 2;

    // If the thing we've hit is underneath us, then we don't want
    // to push it; it would make it impossible for us to walk on
    // top of it, because it would be "pushed."
    if (hit.point.y <= footPosition) {
        return;
    }

    // Apply the push, based on our setting.
    switch (pushMode)
    {
        case PushMode.DirectlySetVelocity:
            // Directly apply the velocity. Less realistic, but can
            // feel better.
```