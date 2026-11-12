
```
// We now have all the information we need to tell the animator
// what to do. Update its parameters; the animator controller
// will play the right animation. This updates the animation's
// root position as well, which we can optionally use to control
// or influence the object's position.

// We're providing three parameters here.
// - 'Moving' is a bool that unambiguously indicates whether we
//   want to be idle or moving.
// - X and Z speed are intended to control a 2D blend tree.
// - Z speed is forward and backward, while X speed is left and
//   right.

// This can also work with a 1D blend tree, where Z speed is the
// only parameter, but if you do this, you should use this
// script in AgentControlsPosition mode.
//

// This is because if your animator controller doesn't have any
// animations that move the root position sideways, the animator
// will find it difficult to make the kinds of turns that the
// agent may try, and the result will be the visible object
// jumping around onscreen as a result of repeatedly moving too
// far from the agent. Play with your blend trees and
// animations to get good-feeling movement.

animator.SetBool(isMovingParameterName, shouldMove);
animator.SetFloat(sidewaysSpeedParameterName, velocity.x);
animator.SetFloat(forwardSpeedParameterName, velocity.y);

// Is the animator controlling our position, instead of
// the agent?
if (mode == Mode.AnimatorControlsPosition) {

// If the animator is controlling our position, the agent
// will start drifting away from the object. If this happens,
// you'll start seeing visual glitches caused by the
// navigation logic not matching the visible object onscreen.

// To fix this, we'll detect if the object is significantly
// far from the agent. "Significantly" means it's more
// than one agent-radius away from the agent's position
// (that is, the object is outside the agent's cylinder).

// When this happens, we'll start bringing the object closer
// to the agent's position. This reduces animation realism
// slightly, because it's movement that's not reflected in
// the motion of the character, but it prevents larger
// errors from accumulating over time.

// Is the object's position far from where the agent wants
// to be?
if (worldDeltaPosition.magnitude > agent.radius)
{
    // Bring us closer to where the agent wants to be
    transform.position = Vector3.Lerp(transform.position,
        agent.nextPosition,
        Time.deltaTime / 0.15f);
}
}
}
```

```
void OnAnimatorMove()
{
    // Which mode is this script in?
    switch (mode)
    {
        case Mode.AgentControlsPosition:
            // Move the object directly where the agent wants
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
```

- 4. Add a NavigationAnimator component to the object.

When the navigation agent starts moving to another location, it will update the parameters of the animator controller.

## Discussion

Making the navigation agent animate as it moves involves a few things. The most important aspect is that, at each frame, a check is performed to see if the mode of the object is the agent controlling its position, or the animator controlling the position. The right animation system is then triggered, depending on the mode.

## 8.7 Cinematic Camera Tracking

### Problem

You want to create a cinematic-style camera, and set it up so that it always keeps a certain object in view.

### Solution

The best way to do this is to use the Cinemachine package. Cinemachine is a procedural camera system designed to provide a cinematic-style camera control system.

> ### WARNING
> 
> If you read Unity's documentation, you might find references to Cinemachine being acquired from the Unity Asset Store. The latest version was moved from the Unity Asset Store to the Unity Package Manager some time ago, but some documentation about Cinemachine includes instructions to fetch it from the Asset Store; do not do this! The version in the Asset Store, while still available, is not the latest, will not be updated, and will not necessarily contain the features you expect or need. Make sure to always fetch a copy of Cinemachine from the Package Manager. For a refresher on the Package Manager, refer to Chapter 1.

First, install the Cinemachine package:

- 1. Open the Window menu, and choose Package Manager.
- 2. In the All tab, locate the Cinemachine package, and click Install.

Next, we'll set up a Cinemachine virtual camera:

- 1. Open the Cinemachine menu, and choose Create Virtual Camera (Figure 8-15). This will create and select a new virtual camera.
- 2. Position the virtual camera somewhere in your scene.
- 3. Drag the object that you want the camera to look at into the virtual camera's Look At slot (Figure 8-16).
- 4. Run the game. The camera will look at the target!

- Cinemachine Window Help
- Create Virtual Camera
- Create FreeLook Camera
- Create Blend List Camera
- Create State-Driven Camera
- Create ClearShot Camera
- Create Dolly Camera with Track
- Create Target Group Camera
- Create Mixing Camera
- Create 2D Camera
- Create Dolly Track with Cart
- Import Example Asset Package
- Import Post Processing V1 Adaptor Asset Package
- Import Post Processing V2 Adaptor Asset Package
- About

Figure 8-15. Creating a virtual camera

### Cinemachine Virtual Camera (Script)

| Property | Value |
| --- | --- |
| Status | Live |
| Game Window Guides | [x] |
| Save During Play | [ ] |
| Priority | 10 |
| Follow | None (Transform) |
| Look At | Cinemachine Character (Transform) |
| Position Blending | Linear |

### Lens

| Property | Value |
| --- | --- |
| Field Of View | 40 |
| Near Clip Plane | 0.1 |
| Far Clip Plane | 5000 |
| Dutch | 0 |

### Body

| Property | Value |
| --- | --- |
| — | Do nothing |

### Aim

| Property | Value |
| --- | --- |
| Composer | |
| Tracked Object Offset | X 0 Y 1.19 Z 0 |
| Lookahead Time | 0 |
| Lookahead Smoothing | 10 |
| Lookahead Ignore Y | [ ] |
| Horizontal Damping | 0.5 |
| Vertical Damping | 0.5 |
| Screen X | 0.45131 |
| Screen Y | 0.50125 |
| Dead Zone Width | 0.17368 |
| Dead Zone Height | 0.1 |
| Soft Zone Width | 0.8 |
| Soft Zone Height | 0.8 |
| Bias X | 0 |
| Bias Y | 0 |

### Noise

| Property | Value |
| --- | --- |
| — | none |

### Extensions

| Property | Value |
| --- | --- |
| Add Extension | (select) |

Figure 8-16. Configuring the virtual camera to look at a target

## Discussion

Cinemachine works with virtual cameras, which are not really cameras: they behave more like a camera operator in the real world, and control the position, orientation, lens settings, and so on, of the real camera. The virtual cameras do no rendering on their own. Cinemachine virtual cameras are controlled by—the somewhat ominous-sounding—Cinemachine brains.

Cinemachine brains connect a real Unity camera with a virtual Cinemachine camera. Because there are likely to be multiple virtual cameras in most cinematic scenes, the brain monitors the situation, based on the priorities you assign it, and blends between cameras as necessary, applying the state of the virtual camera to the real camera, as shown in Figure 8-17.

![](img/4dc1688a9a64dca9b820486c73104ca2_289_0.png)

Figure 8-17. Configuring a camera brain

> ### NOTE
> 
> You can learn more about Cinemachine by reading the Cinemachine documentation, and Unity's tutorial series for Cinemachine.

## 8.8 Automatically Switching Cameras

### Problem

You have multiple cameras in the scene, and you want Cinemachine (see Recipe 8.7) to automatically switch between them so that a target object is always in view.

### Solution

The solution here is to use a Cinemachine ClearShot camera:

- 1. Open the Cinemachine menu, and choose Create ClearShot Camera.

- 2. Select the ClearShot object, and drag the target you want to maintain a view of into the Look At field.

- 3. Add as many virtual cameras as you want as children of the ClearShot group.

### Discussion

A ClearShot is an object that has one or more virtual cameras as its children, and is configured to look at a certain target. It will select from among its different children which camera has the best shot of the target.

> **TIP**
> 
> Learn more about ClearShot cameras in the Unity Scripting API reference.

## 8.9 Keeping Multiple Objects in View

### Problem

You have multiple objects in your scene, and you want a camera to try to keep them all in view at the same time.

### Solution

