```
// them to know where they are in world space.
Vector3 offsetPosition = transform.position;

// If we're playing, our transform is moving, so we need to use
// the cached start position to figure out where our points
// are in world space.
if (Application.isPlaying) {
    offsetPosition = startPosition;
}

Gizmos.color = Color.blue;

// Loop over all the points
for (int p = 0; p < points.Length; p++) {

    // Get this point and the next one, wrapping around to the
    // first
    var p1 = points[p];
    var p2 = points[(p + 1) % points.Length];

    // Draw the point
    Gizmos.DrawSphere(offsetPosition + p1, 0.1f);

    // Draw the line between the points
    Gizmos.DrawLine(offsetPosition + p1, offsetPosition + p2);
}
}
}
```

+   2. Create a new cube, and add a MovingPlatform to it.

+   3. Add as many points to it as you’d like. They’ll be displayed in the Scene view.

+   4. Play the game. The object will move to each of the points, in a circuit.