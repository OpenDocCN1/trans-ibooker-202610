![](bbox=[0.145, 0.093, 0.854, 0.47])

Figure 9-13. An example of a simple track that laps itself

2. Add a vehicle to the scene, and ensure that you're able to drive around (just to make sure that the driving component works).

3. Create a new C# script called Checkpoint.cs, and add the following code to it. Note that it contains two classes; one of them is special, editor-only code that adds buttons to the Inspector to help build the track:

```
#if UNITY_EDITOR
// Include the UnityEditor namespace when this class is being used
// in the Editor.
using UnityEditor;
#endif

public class Checkpoint : MonoBehaviour
{
```