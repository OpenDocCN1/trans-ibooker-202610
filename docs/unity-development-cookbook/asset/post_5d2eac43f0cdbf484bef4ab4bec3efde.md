
![](img/e54c00769db669e3746fa1e50090241f_301_0.png)

Figure 10-1. Displaying the visibility arc

## Discussion

The EnemyVisibility.cs script that we created sets things up so that a given target is visible when it's both within a certain range of the object and in front of the object. At every frame, there's a check to see if the target is visible.

We also created a custom editor for the class we've made, which both visualizes the visibility range in the editor and allows us to edit it without having to go into scripting!

![](img/e54c00769db669e3746fa1e50090241f_301_1.png)

You can learn more about customizing the editor to make your life easier in Chapter 12.

## 10.2 Defining a Path That AI Entities and the Player Can Follow

### Problem

You want to allow entities in your game to find paths around a scene.

### Solution

Navigation meshes are easiest to set up when your level is made of objects that you don’t expect to move during gameplay, such as the ground, walls, and other obstacles. To define a path:

- 1. Select the objects that you don’t expect to move.
- 2. At the top right of the Inspector, click the drop-down arrow and choose Navigation Static (Figure 10-2).

![](img/e54c00769db669e3746fa1e50090241f_302_0.png)

Figure 10-2. Setting a game object to be Navigation Static from the Static menu

With at least one object marked as Navigation Static, you can bake a navigation mesh:

- 1. Open the Window menu, and choose Navigation.
- 2. Go to the Bake tab, and click Bake. A navigation mesh will be generated (Figure 10-3). Note how the mesh stops at the edges of the ground and around the perimeter of the obstacles (which are marked as Navigation Static).

![](img/e54c00769db669e3746fa1e50090241f_303_0.png)

Figure 10-3. A navigation mesh

![](img/e54c00769db669e3746fa1e50090241f_303_1.png)

You can also select the Static checkbox, which will mark the selected objects static for all other purposes as well.

### Discussion

A navigation mesh essentially defines an area of a game environment that can be traversed by entities that have to move around the area. Typically your game world’s navigation mesh will define the parts of a scene that aren’t obstructed. For example, in a forest scene, dense parts of the forest would not be included in your navigation mesh.

You can learn how to use the navigation mesh you’ve created in Recipe 10.3.

## 10.3 Letting Entities in Your Game Follow a Path

### Problem

You want to create an object that can find a path from its current location to a given destination, avoiding walls and other obstacles. You want to test this feature by clicking different places in the world and making the object try to move to where you clicked.

### Solution

To have an object follow a path:

- 1. First, ensure that your scene has a navigation mesh by following the steps in Recipe 10.2.
- 2. Create a new cylinder by opening the GameObject menu and choosing 3D Object → Cylinder.
- 3. Add a NavMeshAgent component to the cylinder.
- 4. Create a new C# script called MoveToPoint.cs, and add the following code to it:

```
using UnityEngine.AI;
// When the player clicks on a part of the world, the NavMeshAgent moves
// to that position.
[RequireComponent(typeof(NavMeshAgent))]
public class MoveToPoint : MonoBehaviour
{
    // The agent we'll be moving around.
    NavMeshAgent agent;

    void Start()
    {
        // Cache a reference to the NavMeshAgent on game start
        agent = GetComponent<NavMeshAgent>();
    }

    // When the user clicks, move the agent.
    void Update()
    {
        // Did the user just click the left mouse button?
        if (Input.GetMouseButtonDown(0))
        {
            // Get the position onscreen, in screen coordinates (i.e.,
            // pixels).
            var mousePosition = Input.mousePosition;

            // Convert this position into a ray that starts at the
            // camera and moves toward where the mouse cursor is.
            var ray = Camera.main.ScreenPointToRay(mousePosition);

            // Store information about any raycast hit in this variable.
            RaycastHit hit;

            // Did the ray hit something?
            if (Physics.Raycast(ray, out hit))
```

```
{
    // Figure out where the ray hit an object.
    var selectedPoint = hit.point;

    // Tell the agent to start moving to that point.
    agent.destination = selectedPoint;
}
}
```

- 5. Add a MoveToPoint component to the cylinder.
- 6. Play the game. Click somewhere in the world, and the cylinder will start moving to the point you clicked.

### Discussion

A NavMeshAgent will do its best to find and follow a useful path to get to where it needs to go, but it might not always succeed. It's a best-effort situation, rather than a guarantee that it'll find a path to the end point.

The inner workings of the navigation system are not actually too complicated once you unpack them. If you're interested in learning more about how it works in Unity, check out the Unity manual.

## 10.4 Finding a Good Distribution of Random Points (Poisson Disc)

### Problem

You want to efficiently find a random collection of points in a given area.

### Solution

Use a Poisson disc distribution:

- 1. Create a new C# script called PoissonDiscSampler.cs, and add the following code to it:

```
// With thanks to Gregory Schlomoff, who wrote this implementation and
// released it into the public domain, and to Robert Bridson of the
// University of British Columbia, for developing the efficient
// algorithm that this code implements.

// http://gregschlom.com/devlog/2014/06/29/
//          Poisson-disc-sampling-Unity.html
// http://www.cs.ubc.ca/~rbridson/docs/
//          bridson-siggraph07-poissondisk.pdf

// Generates a distribution of 2D points that aren't too close to each
// other. Operates in O(N) time.
public class PoissonDiscSampler
{
    // Maximum number of attempts before marking a sample as inactive.
    private const int k = 30;

    // The rectangle in which points will be placed
    private readonly Rect rect;

    // radius squared
    private readonly float radius2;

    // The cell size of the grid of points
    private readonly float cellSize;

    // The grid of points
    private Vector2[,] grid;

    // The list of locations near which we're trying to add new
    // points.
    private List<Vector2> activeSamples = new List<Vector2>();

    // Create a sampler with the following parameters:
    //
    // - width: each sample's x coordinate will be between [0, width]
    // - height: each sample's y coordinate will be between [0, height]
    // - radius: each sample will be at least `radius` units away from
    //   any other sample, and at most 2 * `radius`.
    public PoissonDiscSampler(float width, float height, float radius)
    {
        rect = new Rect(0, 0, width, height);
        radius2 = radius * radius;
        cellSize = radius / Mathf.Sqrt(2);
        grid = new Vector2[Mathf.CeilToInt(width / cellSize),
                          Mathf.CeilToInt(height / cellSize)];
    }

    // Return a lazy sequence of samples. You typically want to call
    // this in a foreach loop, like so:
    //     foreach (Vector2 sample in sampler.Samples()) { ... }
    public IEnumerable<Vector2> Samples()
    {
        // First sample is chosen randomly
        Vector2 firstSample = new Vector2(Random.value * rect.width,
                                          Random.value * rect.height);

yield return AddSample(firstSample);

while (activeSamples.Count > 0) {
    // Pick a random active sample
    int i = (int) Random.value * activeSamples.Count;
    Vector2 sample = activeSamples[i];

    // Try `k` random candidates between [radius, 2 * radius]
    // from that sample.
    bool found = false;
    for (int j = 0; j < k; ++j) {
        float angle = 2 * Mathf.PI * Random.value;
        float r = Mathf.Sqrt(Random.value * 3 * (2 * radius2));
        Vector2 candidate = sample + r * new Vector2(
            Mathf.Cos(angle), Mathf.Sin(angle));

        // Accept candidates if it's inside the rect and farther
        // than 2 * radius to any existing sample.
        if (rect.Contains(candidate) && IsFarEnough(candidate)) {
            found = true;
            yield return AddSample(candidate);
            break;
        }
    }

    // If we couldn't find a valid candidate after k attempts,
    // remove this sample from the active samples queue
    if (!found) {
        activeSamples[i] = activeSamples[activeSamples.Count - 1];
        activeSamples.RemoveAt(activeSamples.Count - 1);
    }
}

private bool IsFarEnough(Vector2 sample)
{
    GridPos pos = new GridPos(sample, cellSize);

    int xmin = Mathf.Max(pos.x - 2, 0);
    int ymin = Mathf.Max(pos.y - 2, 0);
    int xmax = Mathf.Min(pos.x + 2, grid.GetLength(0) - 1);
    int ymax = Mathf.Min(pos.y + 2, grid.GetLength(1) - 1);
```
