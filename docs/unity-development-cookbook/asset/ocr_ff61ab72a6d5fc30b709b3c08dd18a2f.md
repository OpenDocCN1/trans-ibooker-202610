```
// Compute the rotation based on these two angles
Quaternion rotation = Quaternion.Euler(
    elevationToTarget,
    rotationAroundTarget,
    0
);

// Update the distance based on mouse movement
distance = distance - Input.GetAxis("Mouse ScrollWheel") * 5;

// And limit it to the minimum and maximum
distance = Mathf.Clamp(distance, distanceMin, distanceMax);

// Figure out a position that's "distance" units away
// from the target in the reverse direction to where
// we're looking
Vector3 negDistance = new Vector3(0.0f, 0.0f, -distance);
Vector3 position = rotation * negDistance + target.position;

// Update the position
transform.position = position;

// Update the rotation so we're looking at the target
transform.rotation = rotation;
}
}

// Clamps an angle between "min" and "max," wrapping it if it's less
// than 360 degrees or higher than 360 degrees.
public static float ClampAngle(float angle, float min, float max)
{
    // Wrap the angle at -360 and 360
    if (angle < -360F)
        angle += 360F;
    if (angle > 360F)
        angle -= 360F;

    // Clamp this wrapped angle
    return Mathf.Clamp(angle, min, max);
}
}
```

+   3. Add a cube to the scene.
+   4. Select the camera, and drag the cube's entry in the Hierarchy into the Target field.
+   5. Run the game. As you move the mouse, the camera will rotate around the target.