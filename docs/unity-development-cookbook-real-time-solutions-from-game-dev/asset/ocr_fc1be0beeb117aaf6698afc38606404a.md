And the dot product between two vectors at right angles to each other is 0:

```
var orthogonal = Vector3.Dot(Vector3.up, Vector3.forward); // 0
```

As a happy side effect, the dot product between two vectors is also the cosine of the angle between the two vectors. This means that, given the dot product between two vectors, you can calculate the angle between the vectors by taking its arc cosine:

```
var orthoAngle = Mathf.Acos(orthogonal);
var orthoAngleDegrees = orthoAngle * Mathf.Rad2Deg; // = 90
```

> **NOTE**
> 
> The Mathf.Acos method returns a value measured in radians. To convert it to degrees, you can multiply it by the Mathf.Rad2Deg constant.

The dot product is a great way to tell if an object is in front of or behind another.

To tell if one object is in front of another, we first need to decide what “in front of” means. In Unity, the local z-axis represents the forward-facing direction, and you can access it through the forward property on an object’s Transform.

We can produce a vector representing the direction from the first object to the second by subtracting the position of the second from the position of the first. We can then take the dot product of that vector against the forward direction of the first object.

We can now use what we know about the dot product to figure out if the second object is in front of the first. Recall that the dot product of two vectors aiming in the same direction is 1. If the second object is directly in front of the first, then the direction to that object will be identical, which means that the dot product of the two vectors will be 1. If it’s 0, then the