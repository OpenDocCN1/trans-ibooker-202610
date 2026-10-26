

## Lockstep

Lockstep 网络架构通常在 1990 年代末的 RTS 游戏中使用。它是一种网络架构，其中所有玩家都会等待所有其他玩家的承诺。这些承诺是预期操作的哈希版本。一旦玩家收到所有其他承诺，他们就会向所有其他玩家发送他们的操作。然后必须对该操作进行哈希处理并与原始承诺进行比较。

这种网络方法解决了被称为“先看作弊”的一种作弊方式的一些问题。例如，假设你正在玩石头剪刀布游戏，但你无法保证所有玩家会同时出牌。玩家 A 例如可以等待玩家 B 显示其决定，然后显示自己的决定，已知玩家 B 的决定。

在 lockstep 中，玩家 A 和玩家 B 各自共享承诺的哈希版本。此哈希承诺不应用于直接推断操作，但也应易于与操作进行比较。

这样，玩家 A 和玩家 B 就无法对他们的预期操作撒谎。Lockstep 然而带来了一个痛苦的缺点，使其不适合大多数现代游戏。整个网络的速度取决于最慢的玩家，这使其不适合许多游戏和类型。

因此，lockstep 网络的最后一个堡垒可能就是 RTS 游戏。但即使如此，还有另一个缺点。Lockstep 通常需要完全确定性的网络架构，并且由于与 lockstep 网络相关的复杂性和难度，我们通常会放弃它而采用客户端-服务器模型。

## True Lockstep Games

- Supreme Commander 1
- Warcraft 3
- Doom

## Pseudo-lockstep (Lockstep with Client-Server Model)

- Starcraft 2

## Client-Server Architecture

Client-Server Architecture – 这就是我们要介绍的内容。Mirror Networking 使用纯客户端-服务器模型。虽然理论上你可以扩展 Mirror Networking 来使用

RPC 属性可以在属性标志中包含一个特殊的参数，称为“includeOwner”。如果不传递该参数，则默认为 true。如果你决定传递该参数，你的新属性将看起来更像以下内容：

```
[RPC(includeOwner = false)]
public void Jump()
{
    PlayerController.Jump();
}
```

现在，你的 RPC 将仅在客户端上执行并排除主机玩家。通常没有太多理由这样做，但该功能存在。

## TargetRPC Attribute

TargetRPC 与 RPC 属性非常相似。TargetRPC 简单地代表 Target Remote Procedure Call。与 RPC 属性一样，我们需要添加一个属性，这次是 TargetRPC 属性。

TargetRPC 需要一个额外的步骤。我们必须将 NetworkConnection 作为方法的第一个参数包含在内，否则代码将无法工作。让我们看看它是什么样的：

```
[TargetRPC]
public void Jump(NetworkConnection connection)
{
    PlayerController.Jump();
}
```

然后你可以调用远程过程调用并传入你想要在其上调用远程过程的玩家的网络连接。某些 IDE 可能会警告参数未被使用，但这并非如此，你可以安全地忽略此警告。

## Commands

那么，如果我们希望客户端在服务器上执行一个方法呢？我们可以使用命令来实现；它们与 RPCs 非常相似，因为我们可以使用命令属性来识别 Mirror Networking 编译器的目标方法。

## Command Attribute

我们可以像标记方法为 RPCs 或 TargetRPCs 一样将方法标记为命令。重要的是要注意，默认情况下，你必须对你调用命令的网络身份拥有权限，这应该看起来像这样：

```
[Command]
public void AddGold()
{
    gold++;
}
```

给定的命令将允许对网络身份拥有权限的客户端添加金币。然后该 gold 变量可以通过使用 sync-vars 在网络上与其它客户端同步，我们稍后会讨论。

## Synchronization – Properties

Synchronization 是 Mirror Networking 的一个概念，我们可以在网络上同步变量的状态。Mirror Networking 通常在客户端加入网络或值发生变化时执行此同步，并将其添加到同步批处理队列中。

同步涵盖两个主要部分：同步属性和同步组件；后者是预先编写的组件，用于同步其它组件的状态，省去了从头重写大量样板代码的需要，但设计为可扩展以满足项目需求。

我们将从讨论同步属性开始。这些如下：

- SyncVars
- SyncLists
- SyncDictionaries
- SyncHashSets
- SyncSortedSets

然后最后，我们有通常的同步设置。

## Network Lerp Rigidbody

Network Lerp Rigidbody 组件是管理变换复制的替代方案。在你想要在网络上复制非运动学刚体时，它是有优势的。

然而，在刚体上使用 Network Transform 可能会导致物理系统出现异常行为，因为刚体本身的速率不会被修改。为抵消此问题，我们有 Network Lerp Rigidbody。

Network Lerp Rigidbody 同步刚体的速率值及其相关位置。

重要的是要注意，网络身份不应同时具有 Network Transform 和 Network Lerp Rigidbody 组件。你应该根据产品需求选择其一，通常更倾向于 Network Transform，除非速率很重要——例如刚体平台，或玩家可交互的刚体。让我们看一下图 4-13 中的组件。

![](img/9be82dd80844460a2c43cbf8813817d7_141_0.png)

Figure 4-13. The figure shows the Network Lerp Rigidbody component

与 Network Transform Child 类似，我们有一个目标，即我们想要复制的相关刚体。然后，我们有我们的 lerp 设置。这些 lerp 设置决定了组件应尝试在网络同步滴答之间填补空白的程度。

最后，我们有我们熟悉的客户端权限布尔值，它决定谁有权控制网络刚体。

## Network Rigidbody (2D and 3D Variants)

Network Rigidbody 是在网络上同步刚体的另一种方法。然而，这些不会插值其值。对于 2D 和 3D 刚体都有 Network Rigidbody 组件。

其同步属性与我们刚刚介绍的 Network Lerp Rigidbody 组件（图 4-14）有点不同，所以让我们看一下。

这里，我们也有一个可以指定的目标。我们还添加了一些复选框，可以让我们更好地控制应同步哪些属性。

这种额外的控制可以帮助我们在许多情况下减少网络带宽，例如我们可能只想同步速率的移动平台。

![](img/9be82dd80844460a2c43cbf8813817d7_142_0.png)

Figure 4-14. The figure shows the Network Lerp Rigidbody component

