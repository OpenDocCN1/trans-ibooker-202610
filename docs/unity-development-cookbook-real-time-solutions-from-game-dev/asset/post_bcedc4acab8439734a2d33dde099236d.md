
![](img/4dc1688a9a64dca9b820486c73104ca2_273_0.png)

Figure 8-10. The avatar mask, with everything but the character's right arm selected

![](img/4dc1688a9a64dca9b820486c73104ca2_274_0.png)

Figure 8-11. The Arm layer of the animator controller, configured to use the avatar mask

## Discussion

Animation layers can be used to override or add to earlier layers. The different weights that can be set on each animation layer allow you to control how much influence that layer has over the final pose.

You can run multiple animations at the same time by creating multiple layers. Animation masks are used to control which parts of the rig are affected by which layers, and by adjusting the weight of different layers you can allow multiple animations to affect the same parts of the rig.

An avatar is a mapping of common components, such as arms or legs, to specific bones in the animation rig. This allows you to reuse animations across different meshes, even if the bones aren’t quite the same.

## 8.5 Blended Movement

### Problem

You want to blend between multiple animations. For example, you have a walking animation and a running animation, and you want to smoothly blend between the two as a character’s speed increases.

### Solution

Use a *blend tree*, which is an animation state that blends between two or more animation clips:

- 1. Add a floor to the scene, following the same steps as in Recipe 8.2.
- 2. Add the DefaultAvatar object to your scene. Rename it “Blended Movement Character.”
- 3. Open the Assets menu, and choose Create → Animator Controller. Name the new asset “Blended Movement.”
- 4. Select the Blended Movement character, and drag the Blended Movement controller that you just created into the Animator component’s Controller slot.
- 5. With the Blended Movement character still selected, go to the Animator tab by opening the Window menu and choosing Animation → Animator.
- 6. Add a new Float parameter to the controller by going to the Parameters tab, clicking the +, and choosing Float. Name the new parameter “Speed.”
- 7. Right-click in your animator controller, and choose Create State → From New Blend Tree.
- 8. Double-click the new state, and you’ll be taken to the details of the blend tree.
- 9. Select the Blend Tree node, and set its parameter to “Speed.” This is the parameter that the blend tree will use to decide which animation clips to use.
- 10. Drag the following animation clips into the blend tree’s Inspector, and use the specified thresholds and speeds. When you’re done, the Inspector should look like Figure 8-12, and the controller should look like Figure 8-13.
  - WalkFWD; threshold: -1.5, speed: -1
  - Idle_Neutral_1; threshold: 0, speed: 1
  - WalkFWD; threshold: 1.5, speed: -1
  - Run_Impulse; threshold: 5, speed: -1
- 11. Test the game by pressing the Play button. Hold down the up arrow to make the character walk forward. When the character’s speed is 0, the character will stand at idle. As the speed increases, the character will transition through the walking animation to the running animation.

TIP

Adjust the Speed variable in the Character Movement script, and see how it affects the animation.

Inspector

Blend Tree
Blend Type 1D

Parameter Speed

-1.5 5

Motion Threshold

| Motion | Threshold | Speed |
|--------|-----------|-------|
| WalkFWD | -1.5 | -1 |
| Idle_Neutral_1 | 0 | 1 |
| WalkFWD | 1.5 | 1 |
| Run_Impulse | 5 | 1 |

Automate Thresholds
Compute Thresholds Select
Adjust Time Scale Select

Asset Labels

Figure 8-12. The blend tree for the four states

![](img/4dc1688a9a64dca9b820486c73104ca2_278_0.png)

Figure 8-13. The controller for the blend tree

### Discussion

Blend trees work in conjunction with a parameter on the animator controller. Depending on the value of the parameter, the blend tree will blend different animation clips. In Figure 8-12, you can see a blend tree that has four clips, which allow for walking backward, standing still, walking forward, and running forward.

There are multiple types of blend trees, including 2D blend trees. These are useful for when you have two kinds of directions you want to move in (for example, forward/backward and side-to-side). An example of a 2D blend tree can be seen in Figure 8-14; this blend tree uses two parameters, one for forward and reverse motion, and one for sideways motion. When the character is applying only forward motion, the regular running animation is played; when the character is applying only sideways motion, a turning animation is played; when both are applied at once, a running-while-turning animation is played.

![](img/4dc1688a9a64dca9b820486c73104ca2_279_0.png)

Figure 8-14. A 2D blend tree, with two independently controlled parameters

## 8.6 Navigation and Animating in Sync

### Problem

You have a character set up to use a navigation mesh, and you want the character's animation system to respond to the navigation agent moving around the world so that the character looks more realistic.

### Solution

You can get information about how a navigation agent is moving, and use that to provide information to an animator controller's parameters:

- 1. Perform the steps in Recipe 8.5 to set up a character that supports forward/backward and side-to-side movement.
- 2. Set up your character so that it works using a navigation mesh by following the steps in Recipe 10.3.
- 3. Create a new C# script called NavigationAnimator.cs, and add the following code to it:

```csharp
using UnityEngine.AI;

// Uses the NavMeshAgent to drive parameters on the Animator.
[RequireComponent(typeof(NavMeshAgent))]
[RequireComponent(typeof(Animator))]
public class NavigationAnimator : MonoBehaviour
{

// Selects what the final position of the object is controlled by.
enum Mode {
  // The agent directly controls the object's position. More
  // accurate, but causes foot sliding (because the animator isn't
  // moving at the exact speed of the agent).
  AgentControlsPosition,

  // The animator's root motion controls the object's position.
  // Looks better, but less accurate because the motion that
  // results won't precisely match what the agent wants to do.
  AnimatorControlsPosition
}

// The mode that this script is operating in.
[SerializeField] Mode mode = Mode.AnimatorControlsPosition;

// The names of the parameters that this script will control.
[SerializeField] string isMovingParameterName = "Moving";
[SerializeField] string sidewaysSpeedParameterName = "X Speed";
[SerializeField] string forwardSpeedParameterName = "Z Speed";

// Cached references to components we'll be accessing every frame.
Animator animator;
NavMeshAgent agent;

// Stores the movement we did last frame, so that we can smooth out
// our movement over time.
Vector2 smoothDeltaPosition = Vector2.zero;

void Start()
{
    // Cache references to the animator and the nav mesh agent.
    animator = GetComponent<Animator>();
    agent = GetComponent<NavMeshAgent>();

    // The animator will potentially be in charge of our position,
    // not the agent. Disable the agent's ability to directly set
    // this object's position. (The agent will retain the ability
    // to rotate the object.)

    agent.updatePosition = false;
}

void Update()
{
    // The agent stores where it wants to move next in
    // agent.nextPosition.

    // Calculate how far we currently are from where the agent
    // wants to be, in world space.
    Vector3 worldDeltaPosition =
        agent.nextPosition - transform.position;

    // Convert this to local space; we need to know how much of a
    // movement in the x and z planes this would be.
    float xMovement =
        Vector3.Dot(transform.right, worldDeltaPosition);
    float zMovement =
        Vector3.Dot(transform.forward, worldDeltaPosition);

    Vector2 localDeltaPosition = new Vector2(xMovement, zMovement);

    // Smooth out this movement by interpolating from the last
    // frame's movement to this one.
    float smooth = Mathf.Min(1.0f, Time.deltaTime / 0.15f);
    smoothDeltaPosition = Vector2.Lerp(smoothDeltaPosition,
        localDeltaPosition, smooth);

    // Figure out our velocity.
    var velocity = smoothDeltaPosition / Time.deltaTime;

    // We need to tell the animator that we're moving when our
    // velocity exceeds a threshold, and we're not too close
    // to the destination.
    bool shouldMove = velocity.magnitude > 0.5f &&
        agent.remainingDistance > agent.radius;
}
```
