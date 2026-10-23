We can rotate things using degrees, using the Transform class's Rotate method:

```
// Rotate 90 degrees - one quarter circle - around the X axis
transform.Rotate(90, 0, 0);
```

![](bbox=[0.15, 0.21, 0.25, 0.28])

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

# Discussion

The dot product of two unit vectors is equal to the cosine of the angle between them. If you have the cosine of a degree, you can get the original degree by taking the arc cosine of it. This means that you can find the angle between two vectors like this:

```
var angle = Mathf.Acos(Vector3.Dot(Vector3.up, Vector3.left));
```

The result of this is π radians; if you want to show it to the user, you should convert it to degrees first. There are 2π radians in a circle, while there are 360 degrees in a circle; as a result, to convert a number from radians to degrees, you multiply it by 180/π. For example, π/2 radians in degrees = (π/2) * (180/π) = 90. Converting from degrees to radians works in reverse: you multiply it by π/180. For example, 45 degrees in radians is 45 * (π/180) = π/4.

You can simplify this in your code by using the Mathf.Deg2Rad and Mathf.Rad2Deg constants. If you multiply an angle expressed in radians by Mathf.Rad2Deg, you'll get the result in degrees; if you multiply an angle expressed in degrees by Mathf.Deg2Rad, you'll get the result in radians.