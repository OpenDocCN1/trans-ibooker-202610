


```
// Whether this wheel can apply brakes
public bool hasBrakes;
}
```

```csharp
// Controls the power, braking, and steering applied to wheels.
public class Vehicle : MonoBehaviour {

    // The list of wheels on this vehicle
    [SerializeField] Wheel[] wheels = {};

    // The settings used for controlling the wheels:

    // Maximum motor torque
    [SerializeField] float motorTorque = 1000;

    // Maximum brake torque
    [SerializeField] float brakeTorque = 2000;

    // Maximum steering angle
    [SerializeField] float steeringAngle = 45;

    private void Update() {

        // If the vertical axis is positive, apply motor torque and no
        // brake torque. If it's negative, apply brake torque and no
        // motor torque.
        var vertical = Input.GetAxis("Vertical");

        float motorTorqueToApply;
        float brakeTorqueToApply;

        if (vertical >= 0) {
            motorTorqueToApply = vertical * motorTorque;
            brakeTorqueToApply = 0;
        } else {
            // If the vertical axis is negative, cut the engine and step
            // on the brakes.

            // We use Mathf.Abs here to ensure that we use the positive
            // value of "vertical" (because applying negative braking
            // torque would lead to weirdness).
            motorTorqueToApply = 0;
            brakeTorqueToApply = Mathf.Abs(vertical) * brakeTorque;
        }

        // Scale the maximum steering angle by the horizontal axis.
        var currentSteeringAngle =
            Input.GetAxis("Horizontal") * steeringAngle;

        // Update all wheels

        // Using a for loop, rather than a foreach loop, because foreach
        // loops allocate temporary memory, which is turned into garbage
        // at the end of the frame. We want to minimize garbage, because
        // the more garbage that gets generated, the more often the
        // garbage collector has to run, which causes performance
        // problems.
        for (int wheelNum = 0; wheelNum < wheels.Length; wheelNum++) {

            var wheel = wheels[wheelNum];

            // If a wheel is powered, it updates its motor torque
            if (wheel.powered) {
                wheel.collider.motorTorque = motorTorqueToApply;
            }

            // If a wheel is steerable, it updates its steer angle
            if (wheel.steerable) {
                wheel.collider.steerAngle = currentSteeringAngle;
            }

            // If a wheel has brakes, it updates its brake torque
            if (wheel.hasBrakes) {
                wheel.collider.brakeTorque = brakeTorqueToApply;
            }
        }
    }
}
```

- 15. 将 Vehicle.cs 脚本添加到 Vehicle 对象。

- 16. 选择 Vehicle 对象，并向 Wheels 列表添加四个条目。

- 17. 对于每个条目：
  - a. 添加一个轮胎碰撞器。
  - b. 如果是后轮，则打开 Powered。
  - c. 如果是前轮，则打开 Steerable。
  - d. 打开 Has Brakes。

- 18. 运行游戏。你应该能够驾驶汽车四处行驶。

### 讨论

你也可以考虑添加一个环绕摄像机来跟随车辆移动，如配方 9.10 所示。

构建车辆时，考虑当所有车轮都可转向或所有车轮都有动力时会发生什么。

提示

尝试修改车辆质量或扭矩值，看看会发生什么！

在 Unity 中，轮胎碰撞器定义了它们自己的悬架参数。尝试调整它们以获得有趣的效果。

## 9.8 防止汽车翻车

### 问题

你希望汽车在急转弯时不会翻车。

### 解决方案

汽车翻车是因为它正在围绕其过高的质心旋转。当质心较低时，围绕它的任何旋转都会使车轮更紧地压向地面，而不是使整辆汽车翻倒。

要防止汽车翻车：

- 1. 创建一个名为 AdjustCenterOfMass.cs 的新 C# 脚本，并添加以下代码：

```csharp
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

    // Called by the editor to show "gizmos" in the Scene view. Used to
    // help visualize the overridden center of mass.
    private void OnDrawGizmosSelected()
    {
        // Draw a green sphere where the updated center of mass will be.
        Gizmos.color = Color.green;

        var currentCenterOfMass =
            this.GetComponent<Rigidbody>().worldCenterOfMass;

        Gizmos.DrawSphere(currentCenterOfMass + centerOfMass, 0.125f);
    }
}
```

- 2. 将此组件添加到包含车辆 Rigidbody 的游戏对象。

- 3. 将质心的 y 坐标向下移动一点，使其刚好位于车辆地板处。质心越低，车辆越稳定。

### 讨论

在 Unity 中，Rigidbody 通常从其碰撞器计算质心，但你可以覆盖它。

在现实生活中，汽车的质心较低，因为变速箱和发动机等重物位于地板下方。我们在此处的实现方式（以及游戏中典型的实现方式）不够现实，但能提供更好的游戏体验。

## 9.9 创建加速带

### 问题

你希望在赛道上创建一些物体，当车辆驶过它们时会获得临时加速。

### 解决方案

首先，创建加速带的可见组件。在本示例中，我们将采用简单的卡通风格：

- 1. 创建一个新材质，并将其命名为“加速带”。

- 2. 将其反照率颜色设置为红色。

- 3. 打开 GameObject 菜单，选择 3D 对象 → 平面。

- 4. 在赛道上适当定位和缩放该对象。

- 5. 将该对象命名为“加速带”。

- 6. 将“加速带”材质拖到该对象上。

- 7. 从该对象中移除网格碰撞器。

- 8. 添加一个盒形碰撞器。

- 9. 打开 Is Trigger。

- 10. 调整大小和位置，创建足够汽车进入的体积（例如，请参见图 9-12）。

![](img/4dc1688a9a64dca9b820486c73104ca2_350_0.png)

图 9-12. “加速带”对象，带有足够汽车进入的盒形碰撞器

- 11. 创建一个名为 SpeedBoost.cs 的新 C# 脚本，并向其中添加以下代码：

```csharp
public class SpeedBoost : MonoBehaviour {
// The amount of time the boost should apply
[SerializeField] float boostDuration = 1;

// The amount of force to apply in the forward direction
[SerializeField] float boostForce = 50;

// Called when a rigidbody enters the trigger
private void OnTriggerEnter(Collider other)
{
    // Ensure this collider has a rigidbody, either on itself
    // or on a parent object
    var body = other.GetComponentInParent<Rigidbody>();

    if (body == null) {
        return;
    }

    // Attach a ConstantForce component to it
    var boost = body.gameObject.AddComponent<ConstantForce>();

    // Make the ConstantForce boost the object forward by the
    // specified amount
    boost.relativeForce = Vector3.forward * boostForce;

    // Remove this ConstantForce after boostDuration seconds
    Destroy(boost, boostDuration);
}
}
```

- 12. 将 SpeedBoost 组件附加到“加速带”对象。

- 13. 测试游戏。当车辆进入加速区域时，它会向前加速一小段时间。

### 讨论

我们的加速带并不复杂：只是一个带有碰撞器和触发器的对象，并调用物理系统向需要加速的对象施加一些力。加速！

## 9.10 创建围绕目标环绕的摄像机

### 问题

你希望创建一个摄像机，它在固定距离处围绕目标位置旋转。

### 解决方案

要创建环绕摄像机：

- 1. 向场景添加一个摄像机。

- 2. 创建一个名为 OrbitingCamera.cs 的新 C# 脚本，并用以下代码替换其代码：

```csharp
public class OrbitingCamera : MonoBehaviour
{

// The object we're orbiting around
[SerializeField] Transform target;

// The speed at which we change our rotation and elevation
[SerializeField] float rotationSpeed = 120.0f;
[SerializeField] float elevationSpeed = 120.0f;

// The minimum and maximum angle of elevation
[SerializeField] float elevationMinLimit = -20f;
[SerializeField] float elevationMaxLimit = 80f;

// The distance we're at from the target
[SerializeField] float distance = 5.0f;
[SerializeField] float distanceMin = .5f;
[SerializeField] float distanceMax = 15f;

// The angle at which we're rotated around the target
float rotationAroundTarget = 0.0f;

// The angle at which we're looking down or up at the target
float elevationToTarget = 0.0f;

// Use this for initialization
```


