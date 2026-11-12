

![](img/4dc1688a9a64dca9b820486c73104ca2_273_0.png)

图 8-10. 头像遮罩，除角色的右臂外全部选中

![](img/4dc1688a9a64dca9b820486c73104ca2_274_0.png)

图 8-11. 动画控制器的 Arm 层，已配置为使用头像遮罩

## 讨论

动画层可以用于覆盖或叠加到早期层。每个动画层上可设置的不同权重允许你控制该层对最终姿势的影响程度。

通过创建多个层，可以同时运行多个动画。动画掩码用于控制骨架的哪些部分受哪些层影响，通过调整不同层的权重，可以允许多个动画影响骨架的相同部分。

头像是将常见组件（如手臂或腿部）映射到动画骨架中特定骨骼的过程。这样，即使骨骼不完全相同，也可以在不同网格之间重复使用动画。

## 8.5 混合移动

### 问题

你想在多个动画之间进行混合。例如，你有一个行走动画和一个跑步动画，并希望随着角色速度增加，在两者之间平滑混合。

### 解决方案

使用 *混合树*，它是一种在两个或多个动画片段之间进行混合的动画状态：

- 1. 向场景中添加一个地板，步骤与配方 8.2 相同。
- 2. 将 DefaultAvatar 对象添加到你的场景中。将其重命名为“Blended Movement Character。”
- 3. 打开 Assets 菜单，选择 Create → Animator Controller。将新资产命名为“Blended Movement。”
- 4. 选择 Blended Movement 角色，将你刚刚创建的 Blended Movement 控制器拖动到 Animator 组件的 Controller 插槽中。
- 5. 在 Blended Movement 角色仍然被选中的情况下，通过打开 Window 菜单并选择 Animation → Animator，转到 Animator 选项卡。
- 6. 通过进入 Parameters 选项卡，单击 +，并选择 Float，向控制器添加一个新 Float 参数。将新参数命名为“Speed。”
- 7. 在动画控制器中右键单击，选择 Create State → From New Blend Tree。
- 8. 双击新状态，你将被带到混合树的详细信息页面。
- 9. 选择 Blend Tree 节点，并将其参数设置为“Speed。”这是混合树用来决定使用哪些动画片段的参数。
- 10. 将以下动画片段拖动到混合树的 Inspector 中，并使用指定的阈值和速度。完成后，Inspector 应如图 8-12 所示，控制器应如图 8-13 所示。
  - WalkFWD; 阈值: -1.5, 速度: -1
  - Idle_Neutral_1; 阈值: 0, 速度: 1
  - WalkFWD; 阈值: 1.5, 速度: -1
  - Run_Impulse; 阈值: 5, 速度: -1
- 11. 按 Play 按钮测试游戏。按住上箭头键让角色向前行走。当角色速度为 0 时，角色将保持空闲状态。随着速度增加，角色将通过行走动画过渡到跑步动画。

提示

调整 Character Movement 脚本中的 Speed 变量，看看它如何影响动画。

Inspector

混合树
混合类型 1D

参数 Speed

-1.5 5

运动 阈值

| 运动 | 阈值 | 速度 |
|------|------|------|
| WalkFWD | -1.5 | -1 |
| Idle_Neutral_1 | 0 | 1 |
| WalkFWD | 1.5 | 1 |
| Run_Impulse | 5 | 1 |

自动化阈值
计算阈值 Select
调整时间缩放 Select

资产标签

图 8-12. 四个状态的混合树

![](img/4dc1688a9a64dca9b820486c73104ca2_278_0.png)

图 8-13. 混合树的控制器

### 讨论

混合树与动画控制器上的参数协同工作。根据参数的值，混合树将混合不同的动画片段。在图 8-12 中，你可以看到一个包含四个片段的混合树，它们允许向后行走、保持静止、向前行走和向前跑步。

有多种类型的混合树，包括 2D 混合树。当你想要向两种方向移动时（例如，前/后和左右）这些非常有用。图 8-14 中可以看到一个 2D 混合树的示例；这个混合树使用两个参数，一个用于前向和反向运动，另一个用于侧向运动。当角色仅施加前向运动时，将播放常规跑步动画；当角色仅施加侧向运动时，将播放转向动画；当两者同时施加时，将播放边跑步边转向的动画。

![](img/4dc1688a9a64dca9b820486c73104ca2_279_0.png)

图 8-14. 一个 2D 混合树，带有两个独立控制的参数

## 8.6 导航与动画同步

### 问题

你有一个配置为使用导航网格的角色，并希望角色的动画系统能响应导航代理在世界中移动，使角色看起来更逼真。

### 解决方案

你可以获取有关导航代理如何移动的信息，并使用该信息向动画控制器的参数提供数据：

- 1. 执行配方 8.5 中的步骤，设置一个支持前/后和左右移动的角色。
- 2. 按照配方 10.3 中的步骤，设置你的角色以使用导航网格工作。
- 3. 创建一个名为 NavigationAnimator.cs 的新 C# 脚本，并将以下代码添加到其中：

```csharp
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
[SerializeField] string forwardSpeedParameterName = "Z Speed";

// Cached references to components we'll be accessing every frame.
Animator animator;
NavMeshAgent agent;

// Stores the movement we did last frame, so that we can smooth out
// our movement over time.
Vector2 smoothDeltaPosition = Vector2.zero;

void Start()
{
    // Cache references to the animator and the nav mesh agent.
    animator = GetComponent<Animator>();
    agent = GetComponent<NavMeshAgent>();

    // The animator will potentially be in charge of our position,
    // not the agent. Disable the agent's ability to directly set
    // this object's position. (The agent will retain the ability
    // to rotate the object.)

    agent.updatePosition = false;
}

void Update()
{
    // The agent stores where it wants to move next in
    // agent.nextPosition.

    // Calculate how far we currently are from where the agent
    // wants to be, in world space.
    Vector3 worldDeltaPosition =
        agent.nextPosition - transform.position;

    // Convert this to local space; we need to know how much of a
    // movement in the x and z planes this would be.
    float xMovement =
        Vector3.Dot(transform.right, worldDeltaPosition);
    float zMovement =
        Vector3.Dot(transform.forward, worldDeltaPosition);

    Vector2 localDeltaPosition = new Vector2(xMovement, zMovement);

    // Smooth out this movement by interpolating from the last
    // frame's movement to this one.
    float smooth = Mathf.Min(1.0f, Time.deltaTime / 0.15f);
    smoothDeltaPosition = Vector2.Lerp(smoothDeltaPosition,
        localDeltaPosition, smooth);

    // Figure out our velocity.
    var velocity = smoothDeltaPosition / Time.deltaTime;

    // We need to tell the animator that we're moving when our
    // velocity exceeds a threshold, and we're not too close
    // to the destination.
    bool shouldMove = velocity.magnitude > 0.5f &&
        agent.remainingDistance > agent.radius;
}
```

