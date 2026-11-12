

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

