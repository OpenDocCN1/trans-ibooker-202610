
```
end
```

这段代码中第一个检查是为了验证是否存在一个良好的 JWT。我们测试所需要做的就是传入一个已知的错误令牌值。在我们的例子中，“bad_token”就足够了，并且从阅读测试就能清楚地看出我们是如何导致失败的。我们函数的契约是，在这种情况下返回值应仅为原子 :error。我们的测试正在捕获日志，以便测试输出不会因代码中预期的日志而受影响。

包含无效用户 ID 的有效 JWT 的错误情况并没有复杂多少。在同一个 describe 块中，添加这个最后的测试：

```
@tag capture_log: true
test "error: returns :error if user doesn't exist" do
  jwt = sign_jwt(Factory.uuid())

  assert :error = connect(UserSocket, %{token: jwt})
end
```

此测试使用我们的 JWT 辅助函数创建一个包含错误用户 ID 的有效 JWT。我们可以通过为此负载专门创建一个新的 UUID 来保证用户 ID 会是错误的。它不可能存在于数据库中。就像之前的测试一样，我们能够在同一行中同时断言返回 :error 并执行被测代码。同样与之前的测试一样，日志被捕获，因为我们期望代码会产生错误日志。

通过这三个测试，我们现在已经覆盖了调用 connect/3 时可能出现的所有代码路径。UserSocket 模块将用于连接到我们的服务器上的 Channel，我们目前有 LobbyChannel，但也适用于未来可能需要的任何其他 Channel。我们已经覆盖了连接代码。接下来，让我们查看 LobbyChannel 中的代码并学习测试它的细微差别。

## 测试 Channel

我们在 NotSkull 示例应用中提供的 Phoenix Channel 示例 LobbyChannel 功能相当有限。在对其进行测试时，我们不会详尽地展示 Phoenix.ChannelTest 中包含的所有测试宏的用法，但我们会涵盖足够的内容，以便你了解如何使用它们所有。让我们来看一下文件：

testing_phoenix/not_skull/lib/not_skull_web/channels/lobby_channel.ex

```elixir
defmodule NotSkullWeb.LobbyChannel do
  use NotSkullWeb, :channel

  alias NotSkull.GameEngine.Game

  @impl true
  def join("lobby:lobby", _payload, socket) do
    {:ok, socket}
  end

  @spec broadcast_new_game(Game.t()) :: :ok | :error
  def broadcast_new_game(%Game{current_phase: :joining} = game) do
    NotSkullWeb.Endpoint.broadcast!("lobby:lobby", "new_game_created", %{
      game_id: game.id
    })
  end

  def broadcast_new_game(_) do
    :error
  end
end
```

此文件仅提供两个函数，join/3 和 broadcast_new_game/1。join/3 在第 7 行，是 Channel 框架的必需回调，并且简单地返回一个 socket，假设它是用有效参数调用的。因此，我们将在测试的设置部分覆盖此功能。第 13 行是 broadcast_new_game/1，它将广播包含新游戏 ID 的映射到连接到 Channel 的任何客户端。

broadcast_new_game/1 还有一个子句，如果传入的不是 %Game{} 结构体，或者游戏结构体 current_state 的值不是 :joining，则返回错误。我们需要一个 happy path 测试以及针对落入错误子句的每个状态的错误测试。在 test/not_skull_web/channels/lobby_channel_test.exs 创建一个新测试文件，并用以下代码填充测试文件：

```
testing_phoenix/not_skull/test/not_skull_web/channels/lobby_channel_test.bak.exs

1: defmodule NotSkullWeb.LobbyChannelTest do
-   use NotSkullWeb.ChannelCase
-   alias NotSkullWeb.{LobbyChannel, UserSocket}
-
5:   describe "broadcast_new_game/1" do
-   setup do
-     user_id = Factory.uuid()
-
-     {:ok, _, socket} =
        UserSocket
10:
-       |> socket("user_socket:#{user_id}", %{user_id: user_id})
-       |> subscribe_and_join(LobbyChannel, "lobby:lobby")
-
-       %{socket: socket}
    end
15:
-   end
-   end
```

Phoenix Channel 的典型测试将从假设用户已经通过 UserSocket 中的代码连接到 websocket，并且已加入正在测试的 Phoenix Channel 开始。

Phoenix.ChannelTest 提供了两个测试函数，我们可以在 setup 块中使用它们来满足这些条件。

第一个测试函数是 socket/3，我们在第 11 行调用它。它接受的参数是你要连接的 socket 的模块名称，在我们的例子中是 UserSocket，单个 socket 将拥有的 ID，以及应在 assigns 中期望的值。后两个参数需要明确了解 UserSocket 代码的作用。传入的 ID 应该与 UserSocket.id/1 生成的 ID 相同，并且 assigns 需要与 UserSocket.connect/3 中放入其中的内容相匹配。虽然这并不理想，但你会发现 UserSocket 代码通常变化很小。因此，在设置中设置这些值后，很少需要再次修改它们。

一旦 socket 创建完成，我们需要让该 socket 加入实际的 Phoenix Channel 本身并订阅来自 Channel 的事件。名为 subscribe_and_join/3 的函数，在第 12 行调用，提供正是此功能。它需要参数为：由前一个函数 socket/3 创建的 socket、Phoenix Channel 的模块名称，在我们的例子中是 LobbyChannel，以及模块下测试（LobbyChannel）的正确 join/3 函数的主题。我们之前提到过我们不会直接测试 join/3，但它会在我们的设置中被覆盖。这是针对测试函数构建方式的回应。如果你想在 LobbyChannel.join/3 中加入更多逻辑，最好为 join/3 创建一个 describe 块，并在你的执行步骤中使用其中一个 Phoenix.ChannelTest.join 函数（三种不同的 arity，具体取决于你的需求）。在我们的例子中，函数只有一种响应方式，因此专注于它会是过度测试。

在我们进行此调用后，了解“什么”已订阅到 Channel 很重要。当调用 subscribe_and_join/3 时，订阅的是测试进程本身。这意味着测试进程的邮箱是 Channel 消息到达的地方。我们即将描述的测试助手抽象了直接检查邮箱的需求，但了解幕后发生的事情是有用的。

这引导我们为 Channel 中的另一个函数 broadcast_new_game/1 编写测试。在内部，此函数使用 NotSkullWeb.Endpoint.broadcast!/3，它是从 Phoenix.Endpoint 导入的函数。让我们在文件中的 setup 之后添加以下测试，并查看如何测试输出：

```
test "success: returns :ok, sends broadcast when passed an open game" do
  open_game = Factory.struct_for(:game, %{current_phase: :joining})

  assert :ok = LobbyChannel.broadcast_new_game(open_game)

  assert_broadcast("new_game_created", broadcast_payload)
  assert broadcast_payload == %{game_id: open_game.id}

  assert Jason.encode!(broadcast_payload)
end
```

关于此测试需要注意的第一点是，在第 1 行，它没有接受来自 setup 的测试上下文 %{socket: socket}。我们的测试实际上不需要 socket，因为测试进程本身已订阅到 Channel。setup 返回 socket 是为了在我们想要为应用程序添加更多功能，从而添加更多测试时使其可用。

我们被测的函数从不访问数据库，因此我们传入的数据不需要存在于其中。在第 2 行，我们创建一个新游戏，传入对我们正在测试的函数重要的唯一覆盖，即将游戏的 current_phase 设置为 :joining。然后我们在第 4 行执行该函数，断言返回值是 :ok，这恰好是 NotSkullWeb.Endpoint.broadcast!/3 的返回值。被测代码只是将该响应传回测试代码。

被测函数的主要行为不是返回值，而是一个副作用。然而，与其他地方不同，我们需要测试双重来验证副作用是否发生，这里副作用是应该向测试进程的邮箱发送一条消息。如果有其他进程也订阅了同一个 Channel，它们也会收到广播。

在第 6 行，我们的测试调用 assert_broadcast/2。该函数接受参数为预期的事件名称 "new_game_created" 和一个模式，我们传入了一个未绑定的变量。这里通常会固定一个模式，但我们希望捕获整个负载以供后续断言。

我们在第 7 行确实断言负载是我们所期望的。我们测试的最后一个断言需要一点理解。正如你所回忆的，测试进程直接订阅到 Channel。我们的测试旨在关注客户端应用程序的交互，如果 JavaScript 客户端订阅了同一个 Channel。客户端的交互与我们的测试之间存在功能差异。当调用 socket/2 辅助函数时，它创建了一个 socket，跳过了真实的连接过程以提供便利。

如果你查看返回的 socket 中的数据，你会找到一个名为 :serializer 的键，其值为 Phoenix.ChannelTest.NoopSerializer。这个“无操作”序列化器不会以任何方式改变它发送到测试进程的值。如果我们的测试进程旨在代表在应用程序内部运行的另一个 Elixir 进程，这将很棒。然而，在我们的例子中，我们关注的是会发送给 JavaScript 客户端的内容。JavaScript 客户端的 socket 将具有不同的序列化器。在我们的应用程序中
