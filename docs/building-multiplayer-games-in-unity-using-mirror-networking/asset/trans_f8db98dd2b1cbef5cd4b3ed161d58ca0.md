

这涵盖了服务器端代码。它非常易于使用，也便于扩展。

接下来是客户端代码。

```
/// <summary>
/// Called on client from StartClient to initialize the Authenticator
/// <para>Client message handlers should be registered in this method.</para>
/// </summary>
public override void OnStartClient()
{
    // register a handler for the authentication response we expect from
    // server.
    NetworkClient.RegisterHandler<AuthResponseMessage>(OnAuthResponseMessage,
    false);
}
```

同样，与服务器端类似，我们需要注册一个处理器，但这次是针对 `AuthResponseMessage`，以便在收到服务器响应时采取相应操作。我们应该在 `OnStartClient` 中注册这个处理器。

```
/// <summary>
/// Called on client from OnClientAuthenticateInternal when a client needs to
/// authenticate
/// </summary>
/// <param name="conn">Connection of the client.</param>
public override void OnClientAuthenticate(NetworkConnection conn)
{
    AuthRequestMessage authRequestMessage = new AuthRequestMessage();

    NetworkClient.Send(authRequestMessage);
}
```

- **PlayerSpawnMethod** - 指定 Mirror Networking 是在随机的 `NetworkStart` 位置生成玩家，还是以轮询的方式生成。要让此功能正常工作，你需要在场景中放置带有 `NetworkStart` 组件的游戏对象。

## 已注册的可生成预制体

已注册的可生成预制体是一个手动注册的预制体列表，这些预制体允许在运行时生成。这些预制体也可以使用不同的流程自动分配，我们将在本书后面介绍。目前，需要注意的是，如果你打算在游戏过程中实例化并生成某个预制体，而该预制体不在场景文件中，则需要将其注册到此列表中。我们将在本书后面介绍生成网络化对象的内容。

## 网络标识

网络标识是 Mirror Networking 的核心；它们用于在网络中识别对象。如果某样东西需要联网，它就需要一个网络标识。更重要的是，一个网络标识不能成为另一个网络标识的子对象。这一点很重要，因为未来的 Mirror 版本将直接阻止这种情况发生。因此，最好将网络逻辑和网络标识都放在网络化预制体的根节点上。还需要注意，任何存在于场景中的网络标识，在服务器存在并连接之前，都会被 Unity 禁用。这一点很重要，因为在 `Awake` 中执行逻辑会导致逻辑先执行，然后对象被生成，这可能会破坏在 `Awake` 中初始化的一些逻辑，并且不会再次调用 `Awake`。因此，最好避免为附加到网络对象上的任何组件使用 `Awake`。相反，让你的类继承自 `NetworkBehaviour`，并调用 `public void override OnStartClient`。我们将在本书后面更详细地讨论网络行为，深入了解使用 Mirror Networking 创建多人游戏背后的编码。

## 小结

在本章中，我们了解了 Mirror Networking 和不同的网络架构、传输层，以及服务器、客户端和主机。我们还了解了网络管理器以及如何配置多人游戏环境。然后我们深入探讨了身份验证、玩家对象和已注册的可生成预制体。

在下一章中，我们将讨论指挥链，并深入理解权限（authority）以及它如何影响你使用 Mirror 构建多人游戏的方式。

## 练习 1 – 使用 Google 创建自定义 OAuth2 身份验证器

因此，我们打算使用 Google 登录来构建自定义身份验证器。标准的做法是使用 Google 的 SDK；然而，如果你希望开发同时支持桌面平台的产品，你会遇到一个障碍。截至撰写本文时，Google 的 SDK 并不支持原生 Unity 桌面应用程序。

为了为桌面构建目标构建登录流程，你需要使用玩家的浏览器构建一个完全自定义的登录流程，然后拦截 Web 响应。

由于在 Unity 中没有相关指南，更不用说 Mirror 了，我将向你展示如何创建这个自定义登录流程。值得注意的是，同样的概念也适用于 Facebook。然而，这是一个大问题：Facebook 的重定向需要通过 https 重定向到 SSL 加密的端点。这意味着重定向到客户端本地主机是不可行的。如果你打算使用这种方式，你需要托管一个身份验证站点来接收任何重定向。

让我们开始吧。在开始之前，我们需要先设置一些东西：

步骤 1 – 首先，我们需要通过 Google 的 API 开发者门户注册一个新应用。为此，你需要导航到 https://console.cloud.google.com/apis/dashboard 并登录。添加一个应用，然后按照提示操作。

步骤 2 – 接下来，你需要创建一个 API oAuth 2 客户端 ID 以供使用；你可以通过访问 https://console.cloud.google.com/apis/credentials 并确保在顶栏中选择了你的项目（图 3-9）来完成此操作。

那么为什么会有这么多模型呢？关于这一点，可悲的事实是，这直接源于你的一些玩家——作弊者。作弊者总会出现，并且总会抓住机会按自己的规则行事。作弊者作弊是为了自己的利益，或者仅仅是为了享受破坏其他玩家体验所带来的满足感。

游戏服务器架构有助于优化网络的整体设计，以减轻作弊问题，或者在你不在乎作弊者的情况下（例如本地合作游戏）改善玩家体验。因此，架构是一种设计理念，你应该在编写第一行网络代码之前仔细思考，同时权衡时间、精力和回报。

## 输入式与状态式架构

当我们构建多人游戏时，我们究竟在网络中复制什么？有人可能认为这是一个简单的问题，但它完全取决于你的游戏或产品、游戏类型以及架构应该满足的需求。

这就引出了输入式或状态式复制的主题。首先，让我解释一下这究竟是什么意思。当你使用基于输入的复制系统时，我们会通过网络发送玩家的输入，例如“PlayerPressedForwardInput()”命令。

另一方面，如果我们使用状态式架构，我们会通过网络发送位置值，而不是发送移动输入本身。这些方法中的每一种在网络化游戏服务器架构中都有其适用的场景。我们可以在游戏服务器架构中使用输入复制、状态式复制，或两者的组合。这些方法中的每一种在网络化游戏服务器架构中都有其适用的场景。我们的选择取决于网络模拟是否是确定性的。

## 基于输入的复制 – 优点与缺点

通常，这些不同复制形式的优缺点取决于架构本身，因为其中一些可能会变化。然而，一般而言，基于输入的复制具有以下优缺点：

优点

- **通常显著降低带宽使用** - 这是因为你只调用移动和交互命令。此外，在确定性环境中，你甚至不需要复制任何值。

