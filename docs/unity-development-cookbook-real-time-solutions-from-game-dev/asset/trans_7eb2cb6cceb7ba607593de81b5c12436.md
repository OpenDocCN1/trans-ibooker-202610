

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

