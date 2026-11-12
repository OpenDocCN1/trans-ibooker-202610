```
// to be.
transform.position = agent.nextPosition;
break;
case Mode.AnimatorControlsPosition:
// Update our position to where the animation system has
// placed us, following the animation's root movement.

// Override the movement in the y-axis to where the
// agent wants to be (otherwise, we'll just pass through
// hills, stairs, and other changes in ground height).

Vector3 position = animator.rootPosition;
position.y = agent.nextPosition.y;
transform.position = position;
break;
}
}
}
```

+   4. Add a NavigationAnimator component to the object.

When the navigation agent starts moving to another location, it will update the parameters of the animator controller.

# Discussion

Making the navigation agent animate as it moves involves a few things. The most important aspect is that, at each frame, a check is performed to see if the mode of the object is the agent controlling its position, or the animator controlling the position. The right animation system is then triggered, depending on the mode.

# 8.7 Cinematic Camera Tracking