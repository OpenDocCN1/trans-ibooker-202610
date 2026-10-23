+   1. Create a new, empty game object. Name it “Player.”
+   2. Locate the main Camera object. If you don’t have one, open the GameObject menu and choose Camera.
+   3. Drag the Camera object into the Player object, and rename it “Head.”
+   4. Position the Head object where you want the head to be. For best results, don’t modify any component of the position other than the y-component; the others should be at 0. This means that the camera will be directly above the center of the body.
+   5. Create a new C# script called MouseLook.cs, with the following code:

```
// Implements mouseloook. Horizontal mouse movement rotates the body
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
```