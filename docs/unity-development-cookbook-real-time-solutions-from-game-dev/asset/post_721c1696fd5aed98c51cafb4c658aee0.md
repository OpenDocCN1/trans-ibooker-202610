
## Discussion

In your scripts, the Update method is called every frame, which makes it ideal for almost everything that needs to be updated regularly.

Update isn't called at a regular pace; it depends on how long it takes to render each frame. If the scene gets more complex, the number of Update calls per second will change.

By comparison, FixedUpdate is called a fixed number of times per second. This makes it ideal for physics simulation, which behaves better when the rate of time passing between physics simulation updates remains the same.

> NOTE
>
> This doesn't mean that the same amount of real-world time will elapse between FixedUpdate calls; Unity just guarantees that the same number of FixedUpdate calls will happen per second.

If you want to update something at the same rate as physics, put the code in FixedUpdate. If objects that interact with physics objects aren't updated at the same rate as physics objects, you can get odd jittering behaviors, and it doesn't feel good.

## 7.2 Allowing the Player to Look Around Using the Mouse

### Problem

In a first-person-style game, you want to be able to control the direction in which the player is looking by using the mouse.

### Solution

Allowing the player to control a first-person view using their mouse is traditionally referred to as mouselook.

> **NOTE**
> MouseLook is sometimes called *freelook*, as it doesn't have to be tied explicitly to mouse control:
> it can apply to control with a D-pad, a joystick, an analogue stick, or similar.

Implementing mouselook involves rotating two separate objects: the body and the head. The body rotates around the y-axis (also known as the "yaw" axis—the line that goes from down to up), while the head rotates around the x-axis (the line that goes from left to right). In a first-person game, the camera is attached to the head, like this:

- 1. Create a new, empty game object. Name it "Player."
- 2. Locate the main Camera object. If you don't have one, open the GameObject menu and choose Camera.
- 3. Drag the Camera object into the Player object, and rename it "Head."
- 4. Position the Head object where you want the head to be. For best results, don't modify any component of the position other than the y-component; the others should be at 0. This means that the camera will be directly above the center of the body.
- 5. Create a new C# script called *MouseLook.cs*, with the following code:

```csharp
// Implements mouselook. Horizontal mouse movement rotates the body
// around the y-axis, while vertical mouse movement rotates the head
// around the x-axis.

public class MouseLook : MonoBehaviour
{

// The speed at which we turn. In other words, mouse sensitivity.
[SerializeField] float turnSpeed = 90f;

// How far up the head can tilt, measured in angles from dead-
// level. Must be higher than headLowerAngleLimit.
[SerializeField] float headUpperAngleLimit = 85f;

// How far down the head can tilt, measured in angles from dead-
// level. Must be lower than headLowerAngleLimit.
[SerializeField] float headLowerAngleLimit = -80f;

// Our current rotation from our start, in degrees
float yaw = 0f;
float pitch = 0f;

// Stores the orientations of the head and body when the game
// started. We'll derive new orientations by combining these
// with our yaw and pitch.
Quaternion bodyStartOrientation;
Quaternion headStartOrientation;

// A reference to the head object–the object to rotate up and down.
// (The body is the current object, so we don't need a variable to
// store a reference to it.) Not exposed in the interface; instead,
// we'll figure out what to use by looking for a Camera child object
// at game start.
Transform head;

// When the game starts, perform initial setup.
void Start()
{
    // Find our head object
    head = GetComponentInChildren<Camera>().transform;

    // Cache the orientation of the body and head
    bodyStartOrientation = transform.localRotation;
    headStartOrientation = head.transform.localRotation;

    // Lock and hide the cursor
    Cursor.lockState = CursorLockMode.Locked;
    Cursor.visible = false;
}

// Every time physics updates, update our movement. (We do this in
// FixedUpdate to keep pace with physically simulated objects. If
// you won't be interacting with physics objects, you can do
// this in Update instead.)
void FixedUpdate()
{

    // Read the current horizontal movement, and scale it based on
    // the amount of time that's elapsed and the movement speed.
    var horizontal = Input.GetAxis("Mouse X")
        * Time.deltaTime * turnSpeed;

    // Same for vertical.
    var vertical = Input.GetAxis("Mouse Y")
        * Time.deltaTime * turnSpeed;

    // Update our yaw and pitch values.
    yaw += horizontal;
    pitch += vertical;

    // Clamp pitch so that we can't look directly down or up.
    pitch =
        Mathf.Clamp(pitch, headLowerAngleLimit, headUpperAngleLimit);

    // Compute a rotation for the body by rotating around the y-axis
    // by the number of yaw degrees, and for the head around the
    // x-axis by the number of pitch degrees.
    var bodyRotation = Quaternion.AngleAxis(yaw, Vector3.up);
    var headRotation = Quaternion.AngleAxis(pitch, Vector3.right);

    // Create new rotations for the body and head by combining them
    // with their start rotations.
    transform.localRotation = bodyRotation * bodyStartOrientation;
    head.localRotation = headRotation * headStartOrientation;
}
}
```

- 6. Attach a MouseLook component to the body.
- 7. Play the game. Moving the mouse will rotate the body and the head.

## Discussion

By separating the head and body rotation, this approach to orienting the player's perspective avoids the problem of "rolling" the view—something that can happen if an object pitches down and then "yaws" to the side, which produces an orientation that's rotated around the forward axis.

> NOTE
>
> If you prefer your mouselook to be inverted, simply multiply the Input.GetAxis("Mouse Y") value by -1.

## 7.3 Controlling a 3D Character

### Problem

You want to control a character in 3D space.

### Solution

You'll typically want to start with mouselook first, since without the ability to turn, you'll only be able to move forward, backward, and side to side. We'll assume that you've followed the steps in Recipe 7.2.

Once you've implemented mouselook, we'll add some features:

- 1. Add a CharacterController component to your Player object.
- 2. Move the Head object to the top of the CharacterController.
- 3. Create a new C# script called Movement.cs, and add the following code to it:

```csharp
// Implements character controller movement.
public class Movement : MonoBehaviour {

    // The speed at which we can move, in units per second.
    [SerializeField] float moveSpeed = 6;

    // The height of a jump, in units.
    [SerializeField] float jumpHeight = 2;

    // The rate at which our vertical speed will be reduced, in units
    // per second.
    [SerializeField] float gravity = 20;

    // The degree to which we can control our movement while in midair.
    [Range(0, 10), SerializeField] float airControl = 5;

    // Our current movement direction. If we're on the ground, we have
    // direct control over it, but if we're in the air, we only have
    // partial control over it.
    Vector3 moveDirection = Vector3.zero;

    // A cached reference to the character controller, which we'll be
    // using often.
    CharacterController controller;

    void Start()
    {
    controller = GetComponent<CharacterController>();
    }
}

// We do our movement logic in FixedUpdate so that our movement
// can happen at the same pace as physics updates. If it didn't,
// we'd see jitter when we interact with physics objects that can
// move around.
void FixedUpdate () {

// The input vector describes the user's desired local-space
// movement; if we're on the ground, this will immediately
// become our movement, but if we're in the air, we'll
// interpolate between our current movement and this vector, to
// simulate momentum.
var input = new Vector3(
    Input.GetAxis("Horizontal"),
    0,
    Input.GetAxis("Vertical")
);

// Multiply this movement by our desired movement speed
input *= moveSpeed;

// The controller's Move method uses world-space directions, so
// we need to convert this direction to world space
input = transform.TransformDirection(input);

// Is the controller's bottommost point touching the ground?
if (controller.isGrounded)
{
    // Figure out how much movement we want to apply in local
    // space.
    moveDirection = input;
}
}
