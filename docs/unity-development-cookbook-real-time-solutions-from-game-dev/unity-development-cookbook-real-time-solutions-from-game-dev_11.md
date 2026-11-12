# 第 10 章 行为、模拟与 AI

构建引人入胜的世界或游戏，智能的基于决策的行为是核心要素。视频游戏的人工智能与 AI 领域的其他部分不同。它大多是虚假、技巧和复杂性的幻觉。您不需要成为 AI 或机器学习专家，就能为视频游戏构建复杂且引人入胜的行为。在本章中，我们将探讨游戏开发者在为实体赋予行为和 AI 时遇到的一些最常见问题的解决方案。

我们将使用两种不同的技术：行为编程和机器学习。

> **注意**
>
> 您可以使用 Unity 进行真正的 AI 和机器学习开发！本章最后几个示例探讨了使用 Unity ML-Agents，但这是一个庞大的话题。要了解更多信息，请查看我们的书 *机器学习实用模拟*（O'Reilly）。

## 10.1 敌人检测何时能看见玩家

### 问题

您希望对象能够检测何时可以“看见”其目标；也就是说，目标既足够靠近对象，又位于对象前方。您希望能够可视化并配置可以看到目标的区域。

> 提示
>
> 显然，这种技术最常见的用途是确保敌人能找到需要攻击的玩家！

### 解决方案

此解决方案从一个脚本开始：

- 1. 创建一个名为 `EnemyVisibility.cs` 的新脚本，并向其中添加以下代码：

```csharp
#if UNITY_EDITOR
using UnityEditor;
#endif

// Detects when a given target is visible to this object. A target is
// visible when it's both in range and in front of the target. Both the
// range and the angle of visibility are configurable.
public class EnemyVisibility : MonoBehaviour
{
    // The object we're looking for.
    public Transform target = null;

    // If the object is more than this distance away, we can't see it.
    public float maxDistance = 10f;

    // The angle of our arc of visibility.
    [Range(0f, 360f)]
    public float angle = 45f;

    // If true, visualize changes in visibility by changing
    // material color
    [SerializeField] bool visualize = true;

    // A property that other classes can access to determine if we can
    // currently see our target.
    public bool targetIsVisible { get; private set; }

    // Check to see if we can see the target every frame.
    void Update()
    {
        targetIsVisible = CheckVisibility();

        if (visualize) {
            // Update our color: yellow if we can see the target,
            // white if we can't
            var color = targetIsVisible ? Color.yellow : Color.white;

            GetComponent<Renderer>().material.color = color;
        }
    }

    // Returns true if this object can see the specified position.
    public bool CheckVisibilityToPoint(Vector3 worldPoint) {
        // Calculate the direction from our location to the point
        var directionToTarget = worldPoint - transform.position;

        // Calculate the number of degrees from the forward direction.
        var degreesToTarget =
            Vector3.Angle(transform.forward, directionToTarget);

        // The target is within the arc if it's within half of the
        // specified angle. If it's not within the arc, it's not visible.
        var withinArc = degreesToTarget < (angle / 2);

        if (withinArc == false)
        {
            return false;
        }

        // Figure out the distance to the target
        var distanceToTarget = directionToTarget.magnitude;

        // Take into account our maximum distance
        var rayDistance = Mathf.Min(maxDistance, distanceToTarget);

        // Create a new ray that goes from our current location, in the
        // specified direction
        var ray = new Ray(transform.position, directionToTarget);

        // Stores information about anything we hit
        RaycastHit hit;

        // Perform the raycast. Did it hit anything?
        if (Physics.Raycast(ray, out hit, rayDistance)) {
            // We hit something.
            if (hit.collider.transform == target) {
                // It was the target itself. We can see the target point.
                return true;
            }
            // It's something between us and the target. We cannot see
            // the target point.
            return false;
        } else {
            // There's an unobstructed line of sight between us and the
            // target point, so we can see it.
            return true;
        }
    }

    // Returns true if a straight line can be drawn between this object
    // and the target. The target must be within range, and within the
    // visible arc.
    public bool CheckVisibility()
    {
        // Compute the direction to the target
        var directionToTarget = target.position - transform.position;

        // Calculate the number of degrees from the forward direction.
        var degreesToTarget =
            Vector3.Angle(transform.forward, directionToTarget);

        // The target is within the arc if it's within half of the
        // specified angle. If it's not within the arc, it's not visible.
        var withinArc = degreesToTarget < (angle / 2);

        if (withinArc == false) {
            return false;
        }

        // Compute the distance to the point
        var distanceToTarget = directionToTarget.magnitude;

        // Our ray should go as far as the target is or the maximum
        // distance, whichever is shorter
        var rayDistance = Mathf.Min(maxDistance, distanceToTarget);

        // Create a ray that fires out from our position to the target
        var ray = new Ray(transform.position, directionToTarget);

        // Store information about what was hit in this variable
        RaycastHit hit;

        // Records info about whether the target is in range and not
        // occluded
        var canSee = false;

        // Fire the raycast. Did it hit anything?
        if (Physics.Raycast(ray, out hit, rayDistance))
        {
            // Did the ray hit our target?
            if (hit.collider.transform == target)
            {
                // Then we can see it (that is, the ray didn't hit an
                // obstacle between us and the target)
                canSee = true;
            }
            // Visualize the ray.
            Debug.DrawLine(transform.position, hit.point);
        }
        else
        {
            // The ray didn't hit anything. This means that it reached
            // the maximum distance and stopped, which means we didn't
            // hit our target. It must be out of range.

            // Visualize the ray.
            Debug.DrawRay(transform.position,
                directionToTarget.normalized * rayDistance);
        }

        // Is it visible?
        return canSee;

    }
}

#if UNITY_EDITOR
// A custom editor for the EnemyVisibility class. Visualizes and allows
// for editing the visible range.
[CustomEditor(typeof(EnemyVisibility))]
public class EnemyVisibilityEditor : Editor {

// Called when Unity needs to draw the Scene view.
private void OnSceneGUI()
{
    // Get a reference to the EnemyVisibility script we're
    // looking at
    var visibility = target as EnemyVisibility;

    // Start drawing at 10% opacity
    Handles.color = new Color(1, 1, 1, 0.1f);

    // Drawing an arc sweeps from the point you give it. We want to
    // draw the arc such that the middle of the arc is in front of
    // the object, so we'll take the forward direction and rotate
    // it by half the angle.

    var forwardPointMinusHalfAngle =
        // rotate around the y-axis by half the angle
    Quaternion.Euler(0, -visibility.angle / 2, 0)
```


```csharp
// rotate the forward direction by this
// visibility.transform.forward;

// Draw the arc to visualize the visibility arc
Vector3 arcStart =
    forwardPointMinusHalfAngle * visibility.maxDistance;

Handles.DrawSolidArc(
    visibility.transform.position, // Center of the arc
    Vector3.up,              // Up direction of the arc
    arcStart,                // Point where it begins
    visibility.angle,        // Angle of the arc
    visibility.maxDistance   // Radius of the arc
);

// Draw a scale handle at the edge of the arc; if the user drags
// it, update the arc size.

// Reset the handle color to full opacity
Handles.color = Color.white;

// Compute the position of the handle, based on the object's
// position, the direction it's facing, and the distance
Vector3 handlePosition =
    visibility.transform.position +
    visibility.transform.forward * visibility.maxDistance;

// Draw the handle, and store its result.
visibility.maxDistance = Handles.ScaleValueHandle(
    visibility.maxDistance,    // current value
    handlePosition,            // handle position
    visibility.transform.rotation, // orientation
    1,                         // size
    Handles.ConeHandleCap,     // cap to draw
    0.25f);                    // snap to multiples of this
                               // if the snapping key is
                               // held down
}

}

#endif
```

- 2. 在场景中创建两个胶囊体，方法是打开 GameObject 菜单并选择 3D 对象 → 胶囊体。

- 3. 将 EnemyVisibility 组件添加到一个胶囊体上。使其 Target 字段引用另一个胶囊体。请注意，您可以看到可见性弧（图 10-1）。

- 4. 运行游戏。带有 EnemyVisibility 组件的胶囊体在能看到其目标时会变成亮黄色。

![](img/4dc1688a9a64dca9b820486c73104ca2_381_0.png)

图 10-1. 显示可见性弧

### 讨论

我们创建的 EnemyVisibility.cs 脚本设置如下：当给定目标同时位于物体一定范围内且在物体前方时，该目标即可见。每帧都会检查目标是否可见。

我们还为创建的类编写了自定义编辑器，它既可以在编辑器中可视化可见范围，又可以让我们无需编写脚本即可进行编辑！

> 提示
>
> 您可以在第 12 章中了解更多关于自定义编辑器以简化工作的内容。

## 10.2 定义 AI 实体和玩家可遵循的路径

### 问题

您希望游戏中的实体能够在场景中寻找路径。

### 解决方案

当关卡由游戏期间不希望移动的对象组成（例如地面、墙壁和其他障碍物）时，导航网格最容易设置。要定义路径：

- 1. 选择您不希望移动的对象。
- 2. 在检查器右上角，单击下拉箭头并选择 Navigation Static（图 10-2）。

![](img/4dc1688a9a64dca9b820486c73104ca2_384_0.png)

图 10-2. 从静态菜单将游戏对象设置为 Navigation Static

至少有一个对象标记为 Navigation Static 后，您可以烘焙导航网格：

- 1. 打开窗口菜单，并选择导航。
- 2. 转到烘焙选项卡，单击烘焙。将生成一个导航网格（图 10-3）。请注意，网格在地面边缘和障碍物周围停止（这些障碍物已标记为 Navigation Static）。

![](img/4dc1688a9a64dca9b820486c73104ca2_384_1.png)

图 10-3. 导航网格

**注意**

您也可以选择静态复选框，这将使所选对象对所有其他用途也标记为静态。

### 讨论

导航网格本质上定义了游戏环境中实体可遍历的区域。通常，游戏世界的导航网格将定义场景中未被阻挡的部分。例如，在森林场景中，森林的茂密区域不会包含在您的导航网格中。

您可以了解如何在配方 10.3 中使用已创建的导航网格。

## 10.3 让游戏中的实体遵循路径

### 问题

您希望创建一个对象，该对象可以从其当前位置找到通往给定目标的路径，避开墙壁和其他障碍物。您希望通过在世界中单击不同位置，并使对象尝试移动到单击位置来测试此功能。

### 解决方案

要使对象遵循路径：

- 1. 首先，通过遵循配方 10.2 中的步骤，确保您的场景中有导航网格。

- 2. 通过打开 GameObject 菜单并选择 3D 对象 → 圆柱体来创建一个新的圆柱体。

- 3. 将 NavMeshAgent 组件添加到圆柱体。

- 4. 创建一个名为 *MoveToPoint.cs* 的新 C# 脚本，并将以下代码添加到其中：

```csharp
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
            {
                // Figure out where the ray hit an object.
                var selectedPoint = hit.point;

                // Tell the agent to start moving to that point.
                agent.destination = selectedPoint;
            }
        }
    }
}
```

- 5. 将 MoveToPoint 组件添加到圆柱体。

- 6. 运行游戏。在世界中单击某个位置，圆柱体将开始移动到您单击的点。

### 讨论

NavMeshAgent 会尽力查找并遵循一条有用的路径到达目的地，但不一定总能成功。这是一种尽力而为的情况，而非保证能找到通往终点的路径。

一旦拆解开来，导航系统的内部工作原理实际上并不太复杂。如果您有兴趣了解其在 Unity 中的工作原理，请查阅 Unity 手册。

## 10.4 寻找随机点的良好分布（泊松圆盘）

### 问题

您希望在给定区域内高效地找到一组随机点。

### 解决方案

使用泊松圆盘分布：

- 1. 创建一个名为 PoissonDiscSampler.cs 的新 C# 脚本，并将以下代码添加到其中：

```csharp
// With thanks to Gregory Schlomoff, who wrote this implementation and
// released it into the public domain, and to Robert Bridson of the
// University of British Columbia, for developing the efficient
// algorithm that this code implements.

// http://gregschlom.com/devlog/2014/06/29/
// Poisson-disc-sampling-Unity.html
// http://www.cs.ubc.ca/~rbridson/docs/
// bridson-siggraph07-poissondisk.pdf

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
    // foreach (Vector2 sample in sampler.Samples()) { ... }
    public IEnumerable<Vector2> Samples()
    {
        // First sample is chosen randomly
        Vector2 firstSample = new Vector2(Random.value * rect.width,
                                          Random.value * rect.height);

        yield return AddSample(firstSample);

        while (activeSamples.Count > 0) {
```



```csharp
// Pick a random active sample
int i = (int)(Random.value * activeSamples.Count);
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
    if (rect.Contains(candidate) &&
        IsFarEnough(candidate)) {
        found = true;
        yield return AddSample(candidate);
        break;
    }
}

// If we couldn't find a valid candidate after k attempts,
// remove this sample from the active samples queue
if (!found) {
    activeSamples[i] =
        activeSamples[activeSamples.Count - 1];
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

    for (int y = ymin; y <= ymax; y++) {
        for (int x = xmin; x <= xmax; x++) {
            Vector2 s = grid[x, y];
            if (s != Vector2.zero) {
                Vector2 d = s - sample;
                if (d.x * d.x + d.y * d.y < radius2) return false;
            }
        }
    }

    return true;
}

// Note: we use the zero vector to denote an unfilled cell in
// the grid. This means that if we were to randomly pick (0, 0)
// as a sample, it would be ignored for the purposes of
// proximity testing and we might end up with another sample
// too close from (0, 0). This is a very minor issue.
}

// Adds the sample to the active samples queue and the grid before
// returning it
private Vector2 AddSample(Vector2 sample)
{
    activeSamples.Add(sample);
    GridPos pos = new GridPos(sample, cellSize);
    grid[pos.x, pos.y] = sample;
    return sample;
}

// Helper struct to calculate the x and y indices of a sample in the
// grid
private struct GridPos
{
    public int x;
    public int y;

    public GridPos(Vector2 sample, float cellSize)
    {
        x = (int)(sample.x / cellSize);
        y = (int)(sample.y / cellSize);
    }
}
}
```

> **注意**
> 
> 这个脚本不定义新组件。相反，它定义了一个其他脚本将能够使用的类。你不会在添加组件菜单中看到 PoissonDiscSampler 出现，因为它不是 MonoBehaviour 的子类。

- 2. 要测试此功能，请创建一个名为 PoissonDiscDemo.cs 的新 C# 脚本，并将以下代码添加到其中：

```csharp
public class PoissonDiscDemo : MonoBehaviour {

// The area in which we'll place our points
[SerializeField] Vector2 size = new Vector2(10,10);

// The points won't be any closer than this to each other
[SerializeField] float cellSize = 0.5f;

// The list of points we'll show
List<Vector3> points;

// Calculate the points to show when the game starts
private void Awake()
{
    // Create a list of points from the sampler
    points = new List<Vector3>();

    var sampler = new PoissonDiscSampler(size.x, size.y, cellSize);

    foreach (var point in sampler.Samples()) {
        points.Add(
            new Vector3(point.x, transform.position.y, point.y)
        );
    }
}

// Visualize the points we've calculated
private void OnDrawGizmos()
{
    // Early out if we have no list to use
    if (points == null) {
        return;
    }

    Gizmos.color = Color.white;

    // Draw each point in the scene
    foreach (var point in points) {
        Gizmos.DrawSphere(transform.position + point, 0.1f);
    }
}
}
```

- 3. 创建一个空游戏对象并向其添加 PoissonDiscDemo 组件。运行游戏，注意它会如何创建一个随机定位的点阵。还要注意，这些点分布比较均匀——它们不会聚集在一起，也没有稀疏区域（参见图 10-4）。

![](img/4dc1688a9a64dca9b820486c73104ca2_396_0.png)

图 10-4. 泊松分布

## 讨论

随机噪声是游戏开发中非常有用的工具。随机噪声使事物看起来比否则要不那么人工，并可以作为关于寻路等事物决策的基础。

本配方以相对抽象的方式使用随机噪声，但希望你能看到它对游戏开发和设计的用处。我们将在本书的其他一两个配方中探讨随机噪声的其他用途。

## 10.5 敌人检测可掩体位置

### 问题

你想创建一个对象来躲避另一个对象，同时考虑该对象看见事物的能力和周围环境。

### 解决方案

要创建一个对象来躲避另一个对象：

- 1. 创建一个名为 EnemyAvoider.cs 的新 C# 脚本，并将以下代码添加到其中：

```csharp
using UnityEngine.AI;

// Detects if the target can see us, and if it can, navigates to
// somewhere it can't.
[RequireComponent(typeof(NavMeshAgent))]
public class EnemyAvoider : MonoBehaviour {

// The object that's looking for us. We'll use it to determine if it
// can see us, and if it can see the places we're considering hiding.
[SerializeField] EnemyVisibility visibility = null;

// The size of the area where we're considering hiding.
[SerializeField] float searchAreaSize = 10f;

// The density of the search field. Larger numbers means fewer
// hiding places are considered, but it's more efficient.
[SerializeField] float searchCellSize = 1f;

// If true, lines will be drawn indicating where we're considering
// hiding.
[SerializeField] bool visualize = true;

// The navigation agent, which will navigate to the best
// hiding place.
NavMeshAgent agent;

// The Start method is a coroutine; when the game starts, it will
// start a continuous cycle of avoiding the target.
IEnumerator Start()
{
    // Cache a reference to our navigation agent
    agent = GetComponent<NavMeshAgent>();

    // Do this forever:
    while (true) {

        // Can the target see us?
        if (visibility.targetIsVisible) {

            // Find a place to run to where it can't see us anymore.
            Vector3 hidingSpot;

            if (FindHidingSpot(out hidingSpot) == false) {

                // We didn't find anywhere to hide! wait a second
                // and try again.
                yield return new WaitForSeconds(1.0f);
                continue;
            }

            // Tell the agent to start moving to this location
            agent.destination = hidingSpot;
        }

        // Wait a bit, and then check to see if the target can still
        // see us.
        yield return new WaitForSeconds(0.1f);
    }
}
// Attempts to find a nearby place where the target can't see us.
// Returns true if one was found; if it was, put the position in the
// hidingSpot variable.
bool FindHidingSpot(out Vector3 hidingSpot) {

var distribution = new PoissonDiscSampler(
    searchAreaSize, searchAreaSize, searchCellSize);

var candidateHidingSpots = new List<Vector3>();

foreach (var point in distribution.Samples()) {

    var searchPoint = point;

    // Reposition the point so that the middle of the search
    // area is at (0, 0)
    searchPoint.x -= searchAreaSize / 2f;
    searchPoint.y -= searchAreaSize / 2f;

    var searchPointLocalSpace = new Vector3(
        searchPoint.x,
        transform.localPosition.y,
        searchPoint.y
    );

    // Can it see us from here?
    var searchPointWorldSpace =
        transform.TransformPoint(searchPointLocalSpace);

    // Find the nearest point on the navmesh
    NavMeshHit hit;

    bool foundPoint;

    foundPoint = NavMesh.SamplePosition(
        searchPointWorldSpace,
        out hit,
        5,
```



```csharp
NavMesh.AllAreas
);

if (foundPoint == false) {
    // We can't get here. Disregard as a place to hide.
    continue;
}

searchPointWorldSpace = hit.position;

var canSee =
    visibility.CheckVisibilityToPoint(searchPointWorldSpace);

if (canSee == false) {
    // We can't see the target from this position.
    // Return it!
    candidateHidingSpots.Add(searchPointWorldSpace);
}

if (visualize) {
    Color debugColor = canSee ? Color.red : Color.green;

    Debug.DrawLine(
        transform.position, searchPointWorldSpace,
        debugColor, 0.1f);
}

}

if (candidateHidingSpots.Count == 0) {
    // We didn't find a hiding spot.

    // Provide a dummy value
    hidingSpot = Vector3.zero;
    // Indicate our failure
    return false;
}
```

```csharp
// For each of our candidate points, calculate the length of the
// path needed to reach it.

// Build a list of candidate points, matched with the length of
// the path needed to reach it.
List<KeyValuePair<Vector3, float>> paths;

// For each point, calculate the length
paths = candidateHidingSpots.ConvertAll(
    (Vector3 point) => {
    // Create a new path that reaches this point
    var path = new NavMeshPath();
    agent.CalculatePath(point, path);

    // Store the distance needed for this path
    float distance;

    if (path.status != NavMeshPathStatus.PathComplete)
    {
    // If this path doesn't reach the target, consider it
    // infinitely far away
    distance = Mathf.Infinity;
    }
    else
    {
    // Get up to 32 of the points on this path
    var corners = new Vector3[32];
    var cornerCount = path.GetCornersNonAlloc(corners);

    // Start with the first point
    Vector3 current = corners[0];

    distance = 0;

    // Figure out the cumulative distance for each point
    for (int c = 1; c < cornerCount; c++)
    {
        var next = corners[c];
        distance += Vector3.Distance(current, next);
        current = next;
    }

    // Build the pair of point and distance
    return new KeyValuePair<Vector3, float>(point, distance);
});

// Sort this list based on distance, so that the shortest path
// is at the front of the list
paths.Sort((a, b) =>
{
    return a.Value.CompareTo(b.Value);
});

// Return the point that's the shortest to reach
hidingSpot = paths[0].Key;
return true;
}
}
```

- 2. 将 EnemyAvoider.cs 脚本添加到应该躲避目标的对象上。
- 3. 确保目标具有 EnemyVisibility 组件（来自配方 10.1）。
- 4. 将 Visibility 字段设置为目标。
- 5. 运行游戏。当躲避者被目标看到时，它会寻找一个无法被看到的地方并移动过去。在图 10-5 中，红线指向可以被看到的地方，而绿线指向无法被看到的地方。

![](img/4dc1688a9a64dca9b820486c73104ca2_404_0.png)

图 10-5. 躲避者正在寻找躲藏的地方

## 讨论

此算法会不时检查目标是否能看到我们。如果目标能看到我们，算法就会生成附近的一组随机点，并针对每个点判断目标是否能看到该点。任何可见的点都会被丢弃，其余点将继续测试，同样，任何可见的点都会被丢弃。最后，针对每个剩余点计算到达该点所需的路径长度，并选择最短的那条。

> **提示**
>
> 为了获得更好的性能，你可以增大 Search Cell Size 以减少被测试的可能躲藏点数量。如果点数仍然过多，也可以缩小 Search Area Size 以减小所有可能躲藏点的总范围。

## 10.6 构建和使用状态机

### 问题

你想要使用状态机，这是一种编程模式，可以让你管理对象的各种可能状态。

### 解决方案

创建一个名为 `StateMachine.cs` 的新 C# 脚本，并向其中添加以下代码：

```csharp
// Manages a collection of states, which can be transitioned from and to.
public class StateMachine {
    // A single state.
    public class State
    {
        // The state's visible name. Also used to identify the state to
        // the state machine.
        public string name;

        // Called every frame while the state is active.
        public System.Action onFrame;
    }
}
// Called when the state is transitioned to from another state.
public System.Action onEnter;

// Called when the state is transitioning to another state.
public System.Action onExit;

public override string ToString()
{
    return name;
}
}

// The collection of named states.
Dictionary<string, State> states = new Dictionary<string, State>();

// The state that we're currently in.
public State currentState { get; private set; }

// The state that we'll start in.
public State initialState;

// Creates, registers, and returns a new named state.
public State CreateState(string name) {

    // Create the state
    var newState = new State();

    // Give it a name
    newState.name = name;

    // If this is the first state, it will be our initial state
    if (states.Count == 0)
    {
        initialState = newState;
    }

    // Add it to the dictionary
    states[name] = newState;

// And return it, so that it can be further configured
return newState;
}

// Updates the current state.
public void Update() {
// If we don't have any states to use, log the error.
if (states.Count == 0 || initialState == null) {
Debug.LogErrorFormat("State machine has no states!");
return;
}

// If we don't currently have a state, transition to
// the initial state.
if (currentState == null) {
TransitionTo(initialState);
}

// If the current state has an onFrame method, call it.
if (currentState.onFrame != null) {
currentState.onFrame();
}
}

// Transitions to the specified state.
public void TransitionTo(State newState) {
// Ensure we weren't passed null
if (newState == null)
{
Debug.LogErrorFormat("Cannot transition to a null state!");
return;
}

// If we have a current state and that state has an onExit
// method, call it
if (currentState != null && currentState.onExit != null)
{
currentState.onExit();
}
}

Debug.LogFormat(
    "Transitioning from '{0}' to '{1}'", currentState, newState);

// This is now our current state
currentState = newState;

// If the new state has an on enter method, call it
if (newState.onEnter != null)
{
    newState.onEnter();
}
}
}

// Transitions to a named state.
public void TransitionTo(string name) {

    if (states.ContainsKey(name) == false) {
        Debug.LogErrorFormat("State machine doesn't contain a state"
            + " named {0}!", name);

        return;
    }

    // Find the state in the dictionary
    var newState = states[name];

    // Transition to it
    TransitionTo(newState);
}
```

此 StateMachine 类可按如下方式使用：

```csharp
// Demonstrates a state machine. This object has two states: 'searching'
// and 'aiming'. When the target is in range, it transitions from
// 'searching' to 'aiming'; when the target leaves range, it
// transitions back.
public class Turret : MonoBehaviour {

    // The object we'll rotate to aim
    [SerializeField] Transform weapon;

    // The object we're trying to aim at
    [SerializeField] Transform target;

    // Aim at the target when it's within this range
    [SerializeField] float range = 5f;

    // The arc that we'll turn in while the target is out of range
    [SerializeField] float arc = 45;

    // The state machine that manages this object
    StateMachine stateMachine;

    // Use this for initialization
    void Start() {
        // Create the state machine
        stateMachine = new StateMachine();

        // The first state we register will be the initial state
        var searching = stateMachine.CreateState("searching");

        // Log when we enter the state
        searching.onEnter = delegate {
            Debug.Log("Now searching for the target...");
        };

        // Each frame, animate the turret and also check to see if the
        // target is in range
        searching.onFrame = delegate {

            // Sweep from side to side
            var angle = Mathf.Sin(Time.time) * arc / 2f;
            weapon.eulerAngles = Vector3.up * angle;

            // Find the distance to our target
            float distanceToTarget =
                Vector3.Distance(transform.position, target.position);
```



```csharp
// Are they in range?
if (distanceToTarget <= range) {
    // Then transition to the aiming state
    stateMachine.TransitionTo("aiming");
}
};

// The aiming state runs when the target is in range.
var aiming = stateMachine.CreateState("aiming");

// Every frame, keep the turret aimed at the target. Detect
// when the target leaves range.
aiming.onFrame = delegate {

    // Aim the weapon at the target
    weapon.LookAt(target.position);

    // Transition back to 'searching' when it's out of range
    float distanceToTarget =
        Vector3.Distance(transform.position, target.position);

    if (distanceToTarget > range)
    {
        stateMachine.TransitionTo("searching");
    }
};

aiming.onEnter = delegate {
    Debug.Log("Target is in range!");
};

aiming.onExit = delegate {
    Debug.Log("Target went out of range!");
};
}

void Update () {
    // Update the state machine's current state
    stateMachine.Update();
}
}
}
```

## 讨论

状态机非常适合用于需要绝对保证同一时间只能存在一个状态的系统。当你拥有一组固定的状态时——例如，一个可以移动、跳跃或爬行的角色——你绝不希望角色同时处于跳跃和爬行状态。

## 10.7 构建机器学习的仿真环境

## 问题

你希望创建一个包含球体的仿真环境，该球体利用强化机器学习，学习在平台上向特定目标滚动。

## 解决方案

Unity 提供了一个名为 ML-Agents Toolkit 的软件包，它提供了你在 Unity 内部使用真实机器学习来创建和训练仿真所需的一切。解决这个问题的基本步骤如下：

- 1. 在 Unity 中为智能体设计和构建工作环境。这只是一个 Unity 场景！
- 2. 创建一个 Agent，这要求你继承 Agent 类（由 Unity ML-Agents 提供），并为其编写脚本，使其能够观察环境、执行所需操作，并相应地获得奖励。
- 3. 将 Agent 脚本附加到仿真环境中。

要构建环境：

- 1. 创建一个新的 3D 项目，并将其命名为“BallAgent”。
- 2. 使用 Unity Package Manager 将 ML-Agents 包添加到项目中。
- 3. 在场景中添加一个 Plane，为你的球形智能体创建地板。将其命名为“Floor”，并将其 Position 和 Rotation 设置为 (0, 0, 0)，Scale 设置为 (1, 1, 1)。
- 4. 在场景中添加一个 Cube，创建目标。将其命名为“Target”，并将其 Position 设置为 (3, 0.5, 3)，Rotation 设置为 (0, 0, 0)，Scale 设置为 (1, 1, 1)。
- 5. 在场景中添加一个 Sphere，创建将滚向目标的智能体。将其命名为“BallAgent”，并将其 Position 设置为 (0, 0.5, 0)，Rotation 设置为 (0, 0, 0)，Scale 设置为 (1, 1, 1)。
- 6. 使用 Inspector 向球体添加 Rigidbody 组件。
- 7. 在 Hierarchy 中将地板、目标和智能体分组到一个新的 GameObject 下，并将其命名为“Training Area”或类似的名称。

接下来，创建智能体脚本：

- 1. 选择 BallAgent，并在 Inspector 中单击 Add Component 按钮。选择 New Script，将新脚本命名为“BallAgent”，然后单击 Create and Add。
- 2. 打开新脚本，并用以下代码替换：

```csharp
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;
```

```csharp
public class BallAgent : Agent
{
    Rigidbody rBody;
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;

    public override void OnEpisodeBegin()
    {
        // If the Agent fell, zero its momentum
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }
        // Move the target to a new spot
        Target.localPosition = new Vector3(Random.value * 8 - 4,
            0.5f,
            Random.value * 8 - 4);
    }

    public override void CollectObservations(VectorSensor sensor)
    {
        // Target and Agent positions
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        // Agent velocity
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }

    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        // Actions, size = 2
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        // Rewards
        float distanceToTarget = Vector3.Distance(
            this.transform.localPosition, Target.localPosition);

        // Reached target
        if (distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        // Fell off platform
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }

    public override void Heuristic(in ActionBuffers actionsOut)
    {
        var continuousActionsOut = actionsOut.ContinuousActions;
        continuousActionsOut[0] = Input.GetAxis("Horizontal");
        continuousActionsOut[1] = Input.GetAxis("Vertical");
    }
}
```

这个类做了一些有趣的事情：

- 它继承自 ML-Agents 包中提供的 Agent 类，而不是 MonoBehaviour。
- 它扩展了 Agent 类中的四个方法，以便在一个回合开始时知道该做什么（OnEpisodeBegin）、如何收集观察以了解仿真情况（CollectObservations）、如何采取行动（OnActionReceived），以及允许用户驱动智能体（Heuristic）。
- 在 OnEpisodeBegin 中，我们为新回合设置仿真环境。一个回合是智能体尝试解决案例的一个时期，并持续到它成功、失败或时间用尽为止。
- OnEpisodeBegin 通过将目标放置在随机位置来初始化场景，并在智能体在之前的回合中掉落平台时将其重置回平台上。
- CollectObservations 向智能体发送关于其环境的信息。在此示例中，我们发送目标的位置、智能体的位置和智能体的速度。这有助于它到达目标，并帮助它学习操纵速度，以免滚出平台。
- OnActionReceived 接收操作，并为操作分配奖励。这个智能体有两个操作：x 轴上施加的力和 z 轴上施加的力。我们还为其分配一个奖励：如果它到达目标，则获得 1.0 的奖励，我们结束回合；如果它从平台掉落，则不获得奖励地结束回合。
- 最后，Heuristic 允许使用键盘控制在场景中移动智能体以测试仿真。

接下来，我们需要在 Unity 编辑器中设置智能体：

- 1. 选择智能体，并将 Hierarchy 中的 Target 拖入 Inspector 中的 Target 字段（该字段来自我们刚刚创建的脚本）。
- 2. 使用 Add Component 按钮添加 Decision Requester 组件，并将其 Decision Period 设置为 10。
- 3. 使用 Add Component 按钮添加 Behavior Parameters 组件。将其命名为“BallAgent”，并将其 Vector Observation Space Size 设置为 8，Continuous Actions 设置为 2。

我们可以通过在 BallAgent 的 Behavior Parameters 组件中将 Behavior Type 选择为 Heuristic Only 来测试智能体。完成此操作后，按下 Play 并使用方向键在场景中驱动球体智能体。如果你击中了目标，场景（和回合）应将目标重置到新位置。

最后，我们将训练智能体：

- 1. 使用你喜欢的文本编辑器，创建一个名为 BallAgentConfig.yaml 的新文件，并添加以下内容：

```yaml
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
      beta_schedule: constant
      epsilon_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
```



```
gamma: 0.99
strength: 1.0
max_steps: 500000
time_horizon: 64
summary_freq: 10000
```

- 2. 运行 mlagents-learn path/to/BallAgentConfig.yaml --run-id=BallAgent1 来训练代理。

> **提示**
> 可以通过运行以下命令使用 Tensorboard 监控训练过程：
> tensorboard --logdir results --port 6006.

代理完成训练后（这可能需要一些时间，具体取决于您机器的性能），您可以将生成的 .nnx 模型拖动到代理检查器的相应插槽中，并使用训练后的模型再次运行。
