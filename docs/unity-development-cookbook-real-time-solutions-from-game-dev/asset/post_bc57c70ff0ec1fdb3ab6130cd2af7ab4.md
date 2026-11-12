
```
// You can think of a capsule as a cylinder with two spheres
// on either end, where the spheres have the same radius as the
// cylinder. This means that a capsule can be defined by
// three values: the locations of the centers of the two spheres,
// and the radius.
```

```
// Given that a character collider exposes its total height
// (including spheres!) and the radius, we can use this
// to figure out the location of the two capsule points
// in world space.
```

```
// The center of the sphere at the top of the controller's
// capsule
var capsulePoint1 = transform.position + new Vector3(
    0, (controller.height / 2) - controller.radius, 0);
```

```
// The center of the sphere at the bottom of the controller's
// capsule
var capsulePoint2 = transform.position - new Vector3(
    0, (controller.height / 2) + controller.radius, 0);
```

```
// The list of colliders we may be overlapping. We're unlikely
// to overlap more than 10 colliders, so make the list that
// long. (Adjust this if you're encountering lots of overlaps.)
Collider[] overlappingColliders = new Collider[10];
```

```
// Figure out which colliders we're overlapping. We pass in the
// overlappingColliders array, and when this function returns,
// the array will be filled with references to other colliders.
// The function returns the number of colliders that overlap
// the capsule.
var overlapCount = Physics.OverlapCapsuleNonAlloc(
    capsulePoint1, capsulePoint2, // the centers of the spheres
    controller.radius, // the radius of the spheres
    overlappingColliders);

// (Note: we _could_ have used OverlapCapsule, which returns a
// brand-new array, but that requires the function to allocate
// the memory for it on the heap. Because we don't use this
// array after this function ends, the array would turn into
// garbage. More garbage means the garbage collector will run
// more often, which means performance hitches. By creating our
// array locally, we ensure it's stored on the stack; data on
// the stack doesn't get turned into garbage when it goes away,
// but it can't stay around after this function returns,
// which is fine for this case.)

// For each item we were told the capsule overlaps...
for (int i = 0; i < overlapCount; i++) {

    // Get the collider the capsule overlaps
    var overlappingCollider = overlappingColliders[i];

    // If this collider is our controller, ignore it
    if (overlappingCollider == controller) {
        continue;
    }

    // We need to compute how much movement we need to perform
    // to not overlap this collider.

    // First, define some variables to store the direction and
    // distance.
    Vector3 direction;
    float distance;

    // Next, provide information about both our collider and the
    // other one. Our direction and distance variables will be
    // filled with data.
    Physics.ComputePenetration(
        controller, // our collider
        transform.position, // its position
        transform.rotation, // its orientation
        overlappingCollider, // the other collider
        overlappingCollider.transform.position, // its position
        overlappingCollider.transform.rotation, // its
        // orientation
        out direction, // will hold the direction we should
        // move in
        out distance // will contain the distance we should
        // move by
    );

    // Don't get pushed vertically; that's what 1. gravity and
    // 2. moving platforms are for.
    direction.y = 0;

    // Update our position to move out of the way.
    transform.position += direction * distance;
}

// Next, we'll handle standing on a moving platform.

// Cast a ray down to our feet. If it hit a MovingPlatform,
// inherit its velocity.

// (We don't need to worry about avoiding the character
// controller here, because the raycast starts inside the
// controller, so it won't hit it.)

var ray = new Ray(transform.position, Vector3.down);

RaycastHit hit;

// The maximum distance we want to look for.
float maxDistance = (controller.height / 2f) + 0.1f;

// Cast the ray. Did it hit anything?
if (Physics.Raycast(ray, out hit, maxDistance)) {

  // It did!

  // Did it have a MovingPlatform component?
  var platform = hit.collider.gameObject
    .GetComponent<MovingPlatform>();

  if (platform != null) {
    // If it did, update our position based on the
    // platform's current velocity.
    transform.position +=
      platform.velocity * Time.deltaTime;
  }
}
```

- Add a PlatformRiding component to the player character—that is, the object that has your CharacterCollider.

- Play the game, and jump onto an object that has a MovingPlatform component. Your character will be carried with the platform. Additionally, if you get off the platform and then stand in its way, you will be pushed by the platform as it moves into you.

### Discussion

This recipe performs two different tests to see if the CharacterCollider needs to be moved:

- First, it calculates the size of the capsule that the CharacterCollider occupies, and then asks Unity's physics system to check to see what colliders also occupy this space. If a platform is pushing into the character, the two colliders will be intersecting, albeit by a small amount.

If the colliders are intersecting, then we ask the physics system to compute the penetration vector. This is the amount of movement that it would take to move the player so that the two objects aren't overlapping. It's then a simple matter of moving the player by that vector, which means that it moves “out” of the platform; the net result is that the player is pushed away from the platform.

- Next, it performs a raycast from the center of the CharacterCollider down to a short distance below the bottom of the collider. If this raycast hits a Collider that has a MovingPlatform attached to it, the player is moved in the same way as the platform: by multiplying the platform’s current velocity by delta time.

Note that because this code is run in the FixedUpdate method (which means that it’s done at the same rate as all other physics updates), we don’t use Time.deltaTime, but rather Time.fixedDeltaTime—the amount of time that has elapsed since the last call to FixedUpdate.

It’s important to note that because the player is being moved manually, there’s no momentum being imparted. If you’re on a platform that’s moving up quickly and suddenly stops, you won’t be flung into the air due to inertia.

## 7.10 Responding to Being Pushed by Objects

### Problem

You want your character collider to be able to push rigidbodies around.

### Solution

If a character collider is being moved manually, which is the case in the recipes in this chapter, the physics engine won’t automatically apply pushing forces to other objects that the player moves into, even if the object is movable and no matter its mass. Instead, the character collider will notice the overlap and refuse to move into it, which creates the feeling of the object being huge, heavy, and immovable.

To implement pushing, then, we need to manually apply forces, or directly set the velocity. In this recipe, we’ll implement a script that allows for both options:

- Create a new C# script called Pushing.cs. Add the following code to it:

```csharp
// Implements pushing rigidbodies from a character collider.
public class Pushing : MonoBehaviour {

// Defines the possible types of pushing we can apply.
public enum PushMode
{
    // Don't allow any pushing
    NoPushing,

    // Push by directly setting the velocity of things we hit
    DirectlySetVelocity,

    // Push by applying a physical force to the impact point
    ApplyForces
}

// The type of pushing we've selected.
[SerializeField] PushMode pushMode = PushMode.DirectlySetVelocity;

// The amount of force to apply, when push mode is set to
// ApplyForces.
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
    var hitRigidbody = hit.rigidbody;

    // Is this rigidbody something we can push?
    if (hitRigidbody == null || hitRigidbody.isKinematic == true)
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
```
