

![](img/e54c00769db669e3746fa1e50090241f_301_0.png)

图 10-1. 显示可见性弧

## 讨论

我们创建的 EnemyVisibility.cs 脚本设置了这样一种情况：当目标既在物体的一定范围内，又在物体前方时，该目标才可见。每帧都会检查目标是否可见。

我们还为所创建的类创建了一个自定义编辑器，该编辑器既能在编辑器中可视化可见性范围，又允许我们在不进入脚本的情况下对其进行编辑！

![](img/e54c00769db669e3746fa1e50090241f_301_1.png)

你可以在第 12 章了解更多关于自定义编辑器以让你的生活更轻松的内容。

## 10.2 定义 AI 实体和玩家可以跟随的路径

### 问题

你希望允许游戏中的实体在场景中绕行找到路径。

### 解决方案

当你的关卡由游戏过程中不期望移动的物体组成时，例如地面、墙壁和其他障碍物，导航网格最容易设置。要定义路径：

- 1. 选择你不期望移动的物体。
- 2. 在 Inspector 右上角，点击下拉箭头并选择 Navigation Static（图 10-2）。

![](img/e54c00769db669e3746fa1e50090241f_302_0.png)

图 10-2. 从 Static 菜单将游戏对象设置为 Navigation Static

至少标记一个对象为 Navigation Static 后，你可以烘焙导航网格：

- 1. 打开 Window 菜单，并选择 Navigation。
- 2. 进入 Bake 选项卡，点击 Bake。导航网格将被生成（图 10-3）。注意网格如何在地面的边缘和标记为 Navigation Static 的障碍物周围停止。

![](img/e54c00769db669e3746fa1e50090241f_303_0.png)

图 10-3. 一个导航网格

![](img/e54c00769db669e3746fa1e50090241f_303_1.png)

你也可以选择 Static 复选框，这将使所选对象对其他用途也标记为静态。

### 讨论

导航网格本质上定义了游戏环境中可由需要在区域内移动的实体遍历的区域。通常，你的游戏世界的导航网格将定义场景中未被阻挡的部分。例如，在森林场景中，森林的密集部分将不会包含在你的导航网格中。

你可以在配方 10.3 中了解如何使用你创建的导航网格。

## 10.3 让游戏中的实体跟随路径

### 问题

你希望创建一个对象，该对象可以从当前位置找到一条到给定目的地的路径，避免墙壁和其他障碍物。你希望通过点击世界中的不同位置并让对象尝试移动到你点击的位置来测试此功能。

### 解决方案

要让对象跟随路径：

- 1. 首先，通过按照配方 10.2 中的步骤确保你的场景具有导航网格。
- 2. 通过打开 GameObject 菜单并选择 3D Object → Cylinder 创建一个新的圆柱体。
- 3. 向圆柱体添加一个 NavMeshAgent 组件。
- 4. 创建一个新的 C# 脚本名为 MoveToPoint.cs，并添加以下代码：

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

- 5. 向圆柱体添加一个 MoveToPoint 组件。
- 6. 播放游戏。点击世界中的某个位置，圆柱体将开始移动到你点击的位置。

### 讨论

NavMeshAgent 会尽力找到并跟随一条有用的路径到达它需要去的地方，但它可能并不总是成功。这是一种尽力而为的情况，而不是保证它能找到通往终点的路径。

导航系统的内部工作原理一旦拆解后实际上并不太复杂。如果你有兴趣了解它在 Unity 中的工作原理，请查看 Unity 手册。

## 10.4 找到随机点的良好分布（Poisson Disc）

### 问题

你希望在给定区域内高效地找到一组随机点。

### 解决方案

使用 Poisson disc 分布：

- 1. 创建一个新的 C# 脚本名为 PoissonDiscSampler.cs，并添加以下代码到其中：

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
