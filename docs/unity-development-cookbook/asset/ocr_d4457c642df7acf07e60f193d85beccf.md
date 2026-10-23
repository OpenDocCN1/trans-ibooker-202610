point with one that rotates a point won’t have the same result if you combine them in the reverse order.

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

# Discussion

Internally, Unity uses matrices to represent transforms. For simplicity, you spend most of your time directly accessing the position, rotation, and scale. However, knowing how things work under the hood is always a good thing.

# 4.4 Working with Angles

# Problem

You want to work with the angles between vectors.

# Solution

In Unity, most rotations that are represented as Euler angles are given as degrees.