Or use one of Unity’s built-in vectors:

```
var up    = Vector2.up;    // ( 0,  1)
var down  = Vector2.down;  // ( 0, -1)
var left  = Vector2.left;  // (-1,  0)
var right = Vector2.right; // ( 1,  0)
var one   = Vector2.one;   // ( 1,  1)
var zero  = Vector2.zero;  // ( 0,  0)
```

Unity also has a type called Vector3, which is a vector with three dimensions. There are several predefined vectors available from the Vector3 class:

```
Vector3 point = new Vector3(1.0f, 2f, 3.5f);
```

```
var up       = Vector3.up;      // ( 0,  1,  0)
var down     = Vector3.down;    // ( 0, -1,  0)
var left     = Vector3.left;    // (-1,  0,  0)
var right    = Vector3.right;   // ( 1,  0,  0)
var forward  = Vector3.forward; // ( 0,  0,  1)
var back     = Vector3.back;    // ( 0,  0, -1)
var one      = Vector3.one;     // ( 1,  1,  1)
var zero     = Vector3.zero;    // ( 0,  0,  0)
```

![](bbox=[0.14, 0.44, 0.24, 0.52])

You can learn more about Vector2 and Vector3 in Unity’s API documentation.

Every Transform component in Unity has local direction vectors defined, which are relative to their current rotation. For example, an object’s local forward direction can be accessed as:

```
var myForward = transform.forward;
```

Naturally, you can perform basic arithmetic with vectors. Vectors can be added together:

```
var v1 = new Vector3(1f, 2f, 3f);
var v2 = new Vector3(0f, 1f, 6f);

var v3 = v1 + v2; // (1, 3, 9)
```

and subtracted from each other:

```
var v4 = v2 - v1; // (-1, -1, 3)
```

You can also get the magnitude of a vector. Also known as the length of the vector, the vector’s magnitude is the straight-line distance from the origin (0, 0, 0) to the vector. The magnitude of a vector is the square root of the sums of the squares of the components. For example, the magnitude of the vector (0, 2, 0) is 2; the magnitude of the vector (0, 1, 1) is approximately 1.41 (that is, the square root of 2):