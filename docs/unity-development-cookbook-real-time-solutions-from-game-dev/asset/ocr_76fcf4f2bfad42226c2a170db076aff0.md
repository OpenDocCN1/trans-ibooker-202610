+   7. Test your character by pressing the Play button. The character will stand, performing an idle animation.

We're now ready to add inverse kinematics to the animation setup. First, we need to enable IK on the current layer:

+   1. Open your animator controller, and go to the Layers tab.
+   2. Click the Gear icon on the base layer.
+   3. Turn on IK Pass. The layer list should now look like Figure 8-7.

![](bbox=[0.151, 0.321, 0.853, 0.644])

Figure 8-7. Enabling the IK Pass on the animator controller's base layer

Next, we'll create a script that tells the animator controller what to reach for, and what to reach with:

+   1. Create a new C# script called IKReach.cs, and add the following code to it:

```
// Updates the positions and weights used for a specified IK goal.
// The Animator attached to the object this script is on must have a
// layer that has an IK pass enabled. Otherwise, OnAnimatorIK won't
```