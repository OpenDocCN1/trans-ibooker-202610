# 8.6 Navigation and Animating in Sync

# Problem

You have a character set up to use a navigation mesh, and you want the character’s animation system to respond to the navigation agent moving around the world so that the character looks more realistic.

# Solution

You can get information about how a navigation agent is moving, and use that to provide information to an animator controller’s parameters:

+   1. Perform the steps in Recipe 8.5 to set up a character that supports forward/backward and side-to-side movement.
+   2. Set up your character so that it works using a navigation mesh by following the steps in Recipe 10.3.
+   3. Create a new C# script called NavigationAnimator.cs, and add the following code to it:

```
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
```