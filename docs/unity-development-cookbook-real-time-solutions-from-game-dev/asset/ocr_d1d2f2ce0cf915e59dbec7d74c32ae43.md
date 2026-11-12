![](bbox=[0.14, 0.09, 0.86, 0.43])

Figure 9-15. The final checkpoint, placed so that the finish line is between the first and last checkpoint

+   12. Position the vehicle slightly ahead of the Lap Start checkpoint.

+   13. Create a canvas.

+   14. Create and add two game objects with a Text component attached.

+   15. Make one display the text “Wrong Way!” and the other display “Lap 1.”

+   16. Create a new C# script called LapTracker.cs, and add the following code to it:

```
// We use LINQ to help figure out the start of the circuit with fewer
// lines of code. Using LINQ allocates memory, which is something we
// try to avoid, but because we only do it once (at scene start), it's
// not as bad.
using System.Linq;

public class LapTracker : MonoBehaviour {

```