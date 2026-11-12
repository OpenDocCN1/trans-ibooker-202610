```
```csharp
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
```
```