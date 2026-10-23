```
Handles.DrawSolidArc(
    visibility.transform.position, // Center of the arc
    Vector3.up,                     // Up direction of the arc
    arcStart,                       // Point where it begins
    visibility.angle,               // Angle of the arc
    visibility.maxDistance          // Radius of the arc
);

// Draw a scale handle at the edge of the arc; if the user drags
// it, update the arc size.

// Reset the handle color to full opacity
Handles.color = Color.white;

// Compute the position of the handle, based on the object's
// position, the direction it's facing, and the distance
Vector3 handlePosition =
    visibility.transform.position +
    visibility.transform.forward * visibility.maxDistance;

// Draw the handle, and store its result.
visibility.maxDistance = Handles.ScaleValueHandle(
    visibility.maxDistance,        // current value
    handlePosition,                // handle position
    visibility.transform.rotation, // orientation
    1,                             // size
    Handles.ConeHandleCap,         // cap to draw
    0.25f);                        // snap to multiples of this
                                   // if the snapping key is
                                   // held down
    }
}
#endif
```

2. Create two capsules in the scene by opening the GameObject menu and choosing 3D Object → Capsule.

3. Add an EnemyVisibility component to one capsule. Make its Target field refer to the other capsule. Note that you can see the visibility arc (Figure 10-1).

4. Play the game. The capsule with the EnemyVisibility component will turn bright yellow when it can see its target.