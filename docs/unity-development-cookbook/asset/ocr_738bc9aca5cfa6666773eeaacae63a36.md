+   2. Open the Assets menu, and choose Create → Object Color. A new ObjectColour asset is created.
+   3. Select it, and go to the Inspector (Figure 2-5). You’ll find the variables that it exposes, just as you do with MonoBehaviours.
+   4. Set the color in the Inspector.

![](bbox=[0.13, 0.21, 0.86, 0.36])

Figure 2-5. The Inspector for the scriptable ObjectColour asset

We’ll now create a simple script that makes use of an ObjectColour asset. This script will take the color that’s stored in the ObjectColour asset and use it to modify a renderer’s material:

+   1. Create a new C# script called SetColorOnStart.cs, and add the following code to it:

```
public class SetColorOnStart : MonoBehaviour
{
    // The ScriptableObject that we’ll draw data from
    [SerializeField] ObjectColour objectColour;

    private void Update()
    {
        // Don’t try to use the ObjectColour if it hasn’t been provided
        if (objectColour == null)
        {
            return;
        }

        GetComponent<Renderer>().material.color = objectColour.color;
    }
}
```

+   2. Create a cube by opening the GameObject menu and choosing 3D Object → Cube.
+   3. Drag the SetColorOnStart.cs script onto the cube.