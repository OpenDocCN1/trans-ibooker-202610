TIP
Try modifying the mass of the vehicle or the torque values. See what happens!

In Unity, wheel colliders define their own suspension parameters. Play with them for interesting effects.

# 9.8 Keeping a Car from Tipping Over

# Problem

You want your car to not flip over when taking sharp turns.

# Solution

Your car tips over because it’s rotating around its center of mass, which is too high. When the center of mass is lower, any rotation around it will force the wheels into the ground harder, instead of flipping the entire car over.

To keep a car from tipping over:

+   1. Create a new C# script called AdjustCenterOfMass.cs, with the following code:

```
[RequireComponent(typeof(Rigidbody))]
public class AdjustCenterOfMass : MonoBehaviour {

    // How far the center of mass should be moved from its default
    // position
    [SerializeField] Vector3 centerOfMass = Vector3.zero;

    private void Start()
    {
        // Override the center of mass, to enhance stability
        GetComponent<Rigidbody>().centerOfMass += centerOfMass;
    }
```