
In this case, the point has moved from in front of the origin to below it, resulting in the point (0, −1, 0).

If your vector represents a direction, and you want to use a matrix to rotate the vector, you can use MultiplyVector. This method uses only the parts of the matrices that are necessary to do a rotation, so it’s a bit faster:

```
result = rotationMatrix.MultiplyVector(input);
// = (0, -1, 0) - the same result.
```

You can also use a matrix scale a point away from the origin:

```
var scale2x2x2 = Matrix4x4.Scale(new Vector3(2f, 2f, 2f));

var input = new Vector3(1f, 2f, 3f);

var result = scale2x2x2.MultiplyPoint3x4(input);
// = (2, 4, 6)
```

Multiplying matrices together results in a new matrix that, when multiplied with a vector, produces the same result as if you’d multiplied the vector by each of the original matrices in order. In other words, if you think of a matrix as an instruction to modify a point, you can combine multiple matrices into a single step.

![](img/e54c00769db669e3746fa1e50090241f_96_0.png)

When we combine matrices like this, we call it concatenating the matrices.

In this example, we concatenate matrices:

```
var translation = Matrix4x4.Translate(new Vector3(5, 0, 0));
var rotation = Matrix4x4.Rotate(Quaternion.Euler(90, 0, 0));
var scale = Matrix4x4.Scale(new Vector3(1, 5, 1));

var combined = translation * rotation * scale;

var input = new Vector3(1, 1, 1);
var result = combined.MultiplyPoint(input);
Debug.Log(result);
// = (6, 1, 5)
```

As with quaternions, the order of multiplication matters! Matrix multiplication is not commutative, while multiplying regular numbers is. For example, 2 * 5 is the same calculation as 5 * 2: both calculations result in the number 10.

However, moving an object and then rotating it doesn’t produce the same result as rotating an object and then moving it. Likewise, combining a matrix that translates a point with one that rotates a point won’t have the same result if you combine them in the reverse order.

Combining matrices with multiplication will apply them in reverse order of multiplication. Given a point P, and matrices A, B, and C:

```
P * (A * B * C) == (A * (B * (C * P)))
```

you can create a combined translate-rotate-scale matrix using the Matrix4x4.TRS method:

```
var transformMatrix = Matrix4x4.TRS(
    new Vector3(5, 0, 0),
    Quaternion.Euler(90, 0, 0),
    new Vector3(1, 5, 1)
);
```

This new matrix will scale, rotate, and then translate any point you apply it to.

You can also get a matrix that converts a point in the component’s position in local space to world space, which means taking the local position and applying the local translation, rotation, and scaling from this object as well as those of all its parents:

```
var localToWorld = this.transform.localToWorldMatrix;
```

You can also get the matrix that does the reverse—that is, it converts from world space to local space:

```
var worldToLocal = this.transform.worldToLocalMatrix;
```

Phew. That’s a lot of things you can do with matrices!

## Discussion

Internally, Unity uses matrices to represent transforms. For simplicity, you spend most of your time directly accessing the position, rotation, and scale. However, knowing how things work under the hood is always a good thing.

## 4.4 Working with Angles

### Problem

You want to work with the angles between vectors.

### Solution

In Unity, most rotations that are represented as Euler angles are given as degrees.

We can rotate things using degrees, using the Transform class's Rotate method:

```
// Rotate 90 degrees - one quarter circle - around the X axis
transform.Rotate(90, 0, 0);
```

![](img/e54c00769db669e3746fa1e50090241f_98_0.png)

There are 360 degrees in a circle; there are 2π radians in a circle.
They're just different units of measurements for angles.

Degrees are much more familiar to most people, but radians are often easier to calculate with. This is why parts of Unity, particularly those related to math, expect radians. There are 2π radians in a circle:

```
// The sine of pi radians (one half-circle) is zero
Mathf.Sin(Mathf.PI); // = 0
```

You can convert from radians to degrees, and back again, like so:

```
// Converting 90 degrees to radians
var radians = 90 * Mathf.Deg2Rad; // ~= 1.57 (π / 2)
```

```
// Converting 2π radians to degrees
var degrees = 2 * Mathf.PI * Mathf.Rad2Deg; // = 360
```

## Discussion

The dot product of two unit vectors is equal to the cosine of the angle between them. If you have the cosine of a degree, you can get the original degree by taking the arc cosine of it. This means that you can find the angle between two vectors like this:

```
var angle = Mathf.Acos(Vector3.Dot(Vector3.up, Vector3.left));
```

The result of this is π radians; if you want to show it to the user, you should convert it to degrees first. There are 2π radians in a circle, while there are 360 degrees in a circle; as a result, to convert a number from radians to degrees, you multiply it by 180/π. For example, π/2 radians in degrees = (π/2) * (180/π) = 90. Converting from degrees to radians works in reverse: you multiply it by π/180. For example, 45 degrees in radians is 45 * (π/180) = π/4.

You can simplify this in your code by using the Mathf.Deg2Rad and Mathf.Rad2Deg constants. If you multiply an angle expressed in radians by Mathf.Rad2Deg, you'll get the result in degrees; if you multiply an angle expressed in degrees by Mathf.Deg2Rad, you'll get the result in radians.

## 4.5 Finding the Distance to a Target

### Problem

You want to check to see if an object is within a certain range of another.

### Solution

You’ll need to create and add a script to the object that needs to know when the other object is in range of it:

- 1. Create a new C# script called RangeChecker.cs, and add the following code to it:

```
public class RangeChecker : MonoBehaviour {

    // The object we want to check the distance to
    [SerializeField] Transform target;

    // If the target is within this many units of us, it's in range
    [SerializeField] float range = 5;

    // Remembers if the target was in range on the previous frame.
    private bool targetWasInRange = false;

    void Update () {

        // Calculate the distance between the objects
        var distance = (target.position - transform.position).magnitude;

        if (distance <= range && targetWasInRange == false) {
            // If the object is now in range, and wasn't before, log it
            Debug.LogFormat("Target {0} entered range!", target.name);

            // Remember that it's in range for next frame
            targetWasInRange = true;

        } else if (distance > range && targetWasInRange == true) {
            // If the object is not in range, but was before, log it
            Debug.LogFormat("Target {0} exited range!", target.name);

            // Remember that it's no longer in range for next frame
            targetWasInRange = false;
        }
    }
}
```

- 2. Attach this script to any object, and attach any other object to the script’s Target field, and the script will detect when the target enters and exits the specified range.

## Discussion

If you combine this recipe with Recipe 4.6, you can pretty easily put together a behavior in which an object can only “see” nearby objects that are in front of it. A more sophisticated version of this script can be seen in Recipe 10.1.

## 4.6 Finding the Angle to a Target

### Problem

You want to find the angle between two objects.

### Solution

You’ll need to create and add a script to the object that needs to know the angle between it and another object:

- 1. Create a new C# script called AngleChecker.cs, and add the following code to it:

```
public class AngleChecker : MonoBehaviour {

    // The object we want to find the angle to
    [SerializeField] Transform target;

    void Update () {

        // Get the normalized direction to the target
        var directionToTarget =
            (target.position - transform.position).normalized;

        // Take the dot product between that direction and our forward
        // direction
        var dotProduct = Vector3.Dot(transform.forward,
                                    directionToTarget);

        // Get the angle
        var angle = Mathf.Acos(dotProduct);
```
