

+   1. 创建一个新的、空的游戏对象。将其命名为“Player。”
+   2. 找到主 Camera 对象。如果没有，请打开 GameObject 菜单并选择 Camera。
+   3. 将 Camera 对象拖入 Player 对象中，并将其重命名为“Head。”
+   4. 将 Head 对象放置在你想要头部所在的位置。为获得最佳效果，不要修改位置的任何分量，除了 y 分量；其他分量应为 0。这意味着摄像机将直接位于身体中心的正上方。
+   5. 创建一个新的 C# 脚本，命名为 MouseLook.cs，并添加以下代码：

```
// Implements mouseloook. Horizontal mouse movement rotates the body
// around the y-axis, while vertical mouse movement rotates the head
// around the x-axis.
public class MouseLook : MonoBehaviour
{

    // The speed at which we turn. In other words, mouse sensitivity.
    [SerializeField] float turnSpeed = 90f;

    // How far up the head can tilt, measured in angles from dead-
    // level. Must be higher than headLowerAngleLimit.
    [SerializeField] float headUpperAngleLimit = 85f;

    // How far down the head can tilt, measured in angles from dead-
    // level. Must be lower than headLowerAngleLimit.
    [SerializeField] float headLowerAngleLimit = -80f;

    // Our current rotation from our start, in degrees
    float yaw = 0f;
    float pitch = 0f;

    // Stores the orientations of the head and body when the game
    // started. We'll derive new orientations by combining these
    // with our yaw and pitch.
    Quaternion bodyStartOrientation;
    Quaternion headStartOrientation;

    // A reference to the head object–the object to rotate up and down.
    // (The body is the current object, so we don't need a variable to
    // store a reference to it.) Not exposed in the interface; instead,
    // we'll figure out what to use by looking for a Camera child object
    // at game start.
    Transform head;

    // When the game starts, perform initial setup.
    void Start()
    {
```

```
// Find our head object
head = GetComponentInChildren<Camera>().transform;

// Cache the orientation of the body and head
bodyStartOrientation = transform.localRotation;
headStartOrientation = head.transform.localRotation;

// Lock and hide the cursor
Cursor.lockState = CursorLockMode.Locked;
Cursor.visible = false;
}

// Every time physics updates, update our movement. (We do this in
// FixedUpdate to keep pace with physically simulated objects. If
// you won't be interacting with physics objects, you can do
// this in Update instead.)
void FixedUpdate()
{
    // Read the current horizontal movement, and scale it based on
    // the amount of time that's elapsed and the movement speed.
    var horizontal = Input.GetAxis("Mouse X")
                    * Time.deltaTime * turnSpeed;

    // Same for vertical.
    var vertical = Input.GetAxis("Mouse Y")
                   * Time.deltaTime * turnSpeed;

    // Update our yaw and pitch values.
    yaw += horizontal;
    pitch += vertical;

    // Clamp pitch so that we can't look directly down or up.
    pitch =
        Mathf.Clamp(pitch, headLowerAngleLimit, headUpperAngleLimit);

    // Compute a rotation for the body by rotating around the y-axis
    // by the number of yaw degrees, and for the head around the
    // x-axis by the number of pitch degrees.
    var bodyRotation = Quaternion.AngleAxis(yaw, Vector3.up);
    var headRotation = Quaternion.AngleAxis(pitch, Vector3.right);

    // Create new rotations for the body and head by combining them
    // with their start rotations.
    transform.localRotation = bodyRotation * bodyStartOrientation;
    head.localRotation = headRotation * headStartOrientation;
}
}
```

+   6. 将 MouseLook 组件附加到身体上。
+   7. 播放游戏。移动鼠标将旋转身体和头部。

## Discussion

通过分离头部和身体的旋转，这种定向玩家视角的方法避免了“滚动”视图的问题——如果一个物体向下俯仰然后向侧面“偏航”，就会发生这种情况，这会产生绕前轴旋转的朝向。

![](img/e54c00769db669e3746fa1e50090241f_177_0.png)

如果你希望你的 mouseloook 为反向，只需将 Input.GetAxis("Mouse Y") 的值乘以 −1 即可。

## 7.3 控制 3D 角色

## Problem

你想要在 3D 空间中控制一个角色。

## Solution

你通常希望先实现 mouseloook，因为如果没有转向能力，你只能向前、向后和左右移动。我们假设你已按照 Recipe 7.2 中的步骤操作。

一旦你实现了 mouseloook，我们将添加一些功能：

+   1. 在你的 Player 对象上添加一个 CharacterController 组件。
+   2. 将 Head 对象移动到 CharacterController 的顶部。
+   3. 创建一个新的 C# 脚本，命名为 Movement.cs，并向其中添加以下代码：

```
// Implements character controller movement.
public class Movement : MonoBehaviour {

    // The speed at which we can move, in units per second.
    [SerializeField] float moveSpeed = 6;

    // The height of a jump, in units.
    [SerializeField] float jumpHeight = 2;

    // The rate at which our vertical speed will be reduced, in units
    // per second.
    [SerializeField] float gravity = 20;

```

```
// The degree to which we can control our movement while in midair.
[Range(0, 10), SerializeField] float airControl = 5;

// Our current movement direction. If we're on the ground, we have
// direct control over it, but if we're in the air, we only have
// partial control over it.
Vector3 moveDirection = Vector3.zero;

// A cached reference to the character controller, which we'll be
// using often.
CharacterController controller;

void Start()
{
    controller = GetComponent<CharacterController>();
}

// We do our movement logic in FixedUpdate so that our movement
// can happen at the same pace as physics updates. If it didn't,
// we'd see jitter when we interact with physics objects that can
// move around.
void FixedUpdate () {

    // The input vector describes the user's desired local-space
    // movement; if we're on the ground, this will immediately
    // become our movement, but if we're in the air, we'll
    // interpolate between our current movement and this vector, to
    // simulate momentum.
    var input = new Vector3(
        Input.GetAxis("Horizontal"),
        0,
        Input.GetAxis("Vertical")
    );

    // Multiply this movement by our desired movement speed
    input *= moveSpeed;

    // The controller's Move method uses world-space directions, so
    // we need to convert this direction to world space
    input = transform.TransformDirection(input);

    // Is the controller's bottommost point touching the ground?
    if (controller.isGrounded)
    {
        // Figure out how much movement we want to apply in local
        // space.
        moveDirection = input;

        // Is the user pressing the jump button right now?
```

```
if (Input.GetButton("Jump"))
{
    // Calculate the amount of upward speed we need,
    // considering that we add moveDirection.y to our height
    // every frame, and we reduce moveDirection.y by gravity
    // every frame.
    moveDirection.y = Mathf.Sqrt(2 * gravity * jumpHeight);
} else {
    // We're on the ground, but not jumping. Set our
    // downward movement to 0 (otherwise, because we're
    // continuously reducing our y-movement, if we walk off
    // a ledge, we'd suddenly have a huge amount of
    // downward momentum).
    moveDirection.y = 0;
}
} else {
    // Slowly bring our movement toward the user's desired
    // input, but preserve our current y-direction (so that the
    // arc of the jump is preserved)
    input.y = moveDirection.y;
    moveDirection = Vector3.Lerp(moveDirection, input,
        airControl * Time.deltaTime);
}

// Bring our movement down by applying gravity over time
moveDirection.y -= gravity * Time.deltaTime;

// Move the controller. The controller will refuse to move into
// other colliders, which means that we won't clip through the
// ground or other colliders. (However, this doesn't stop other
// colliders from moving into us. For that, we'd need to detect
// when we're overlapping another collider, and move away from
// them. We'll cover this in another recipe!)
controller.Move(moveDirection * Time.deltaTime);
}
```

+   4. 将 Movement 组件附加到你的 Player 对象上。

+   5. 播放游戏。按下方向键或 W、A、S 和 D 键将移动玩家（除非你已自定义输入系统）。你也可以通过按空格键跳跃。

## Discussion

完全可以为你的游戏实现一个完全物理正确的控制设置，其中玩家被模拟为刚体并通过对该物体施加力来移动。然而，为了获得最佳手感的控制，在游戏中最好使用自己的代码来管理玩家的移动，而不是

