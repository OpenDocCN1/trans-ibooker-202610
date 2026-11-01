
```
test "when the process \"ticks\", the Twilio interface is called" do
  interval_in_ms = 5
  phone_number = "+1 111 11 1111"
  test_pid = self()
  ref = make_ref()

  stub(SoggyWaffle.TwilioMock, :send_sms, fn to, text ->
    assert to == phone_number
    # TODO: assert on text
    send(test_pid, {:send_sms_called, ref})
    :ok
  end)

  start_options = [
    interval: interval_in_ms,
    city: "Los Angeles",
    phone_number: phone_number
  ]

  start_supervised!({SoggyWaffle.WeatherChecker, start_options})

  assert_receive {:send_sms_called, ^ref}
  end
  end
```

包含一个引用（使用 make_ref/0 生成）作为此类消息是一种良好的实践，这样我们可以确信我们断言的消息正是我们想要断言的那条消息。原因在于引用是足够唯一的项（在实际用途中每个引用大约在调用 make_ref/0 2⁸² 次后才会再次出现）。我们想强调，上述的消息发送技术在覆盖异步代码的 Elixir 测试中相当常见。并非总有更好的替代方案，因此它是我们工具箱中值得拥有的好工具。

这个 GenServer 及其测试都有效。然而，测试存在一个问题：如果我们重构 GenServer 使其在 tick 时发送两条或更多消息，测试仍会愉快地通过。这是由于我们使用了 Mox.stub/3，它允许你尽可能多次调用 stub 函数，并且不会对调用次数进行断言。让我们在下一节尝试修复这一不足。

## 在黑盒模型与实际需求之间取得平衡

Mox 提供了我们想要的完美函数：Mox.expect/4。我们可以使用 expect/4 来确保 send_sms/2 函数恰好被调用一次。然而，由于测试对象本身的特性，我们的问题仍然存在。由于 GenServer 周期性地执行其操作，我们在测试中并不知道它会 tick 多少次。例如，如果机器特别慢，那么在单个测试过程中，GenServer 可能会 tick 几次，尤其是当我们像上一节那样使用快速间隔时。因此问题出在测试的基础之上。

事实证明，我们很难摆脱目前所处的困境。稍后我们会看到，这一情况很好地概括了一类与周期性操作相关的测试问题。在这本书中，我们始终努力严格遵循测试原则，例如将应用程序代码视为黑盒，以及避免仅为了使测试更容易而对应用程序代码进行更改。然而，我们之前已经稍微打破了规则：例如，在编译时读取 @twilio_module 属性以便在测试期间交换 Twilio 接口替身，这无疑是我们为了有利于测试而对应用程序代码所做的更改。在这种情况下，我们将采取类似但侵入性稍强的方法，对 SoggyWaffle.WeatherChecker 进行逻辑更改以使测试更容易。我们在追求干净、实用且同时有效的测试之间走钢丝。

我们面临的基本问题是，我们的 GenServer 在后台执行周期性操作，一旦启动就会一直重复，直到我们停止 GenServer 本身。那么，如果我们不周期性地执行操作，而是“按需”执行呢？我们当前的 SoggyWaffle.WeatherChecker 不支持这一点，但这是我们可以更改的。我们将在启动 GenServer 时增加对 :mode 选项的支持。此选项控制 GenServer 是否像现在这样周期性地执行 tick（:periodic 模式），还是我们手动触发每次 tick（:manual 模式）。

```
testing_otp/periodic_actions/weather_checker_with_mode.ex

defmodule SoggyWaffle.WeatherChecker do
  use GenServer

  # same module attribute and start_link/1 as before

  @impl GenServer
  def init(opts) do
    mode = Keyword.get(opts, :mode, :periodic)
    interval = Keyword.fetch!(opts, :interval)

    state = %{
      city: Keyword.fetch!(opts, :city),
      phone_number: Keyword.fetch!(opts, :phone_number),
    }

    case mode do
      :periodic ->
        :timer.send_interval(interval, self(), :tick)

      :manual ->
        :ok
    end

    {:ok, state}
  end

  @impl GenServer
  def handle_info(:tick, state) do
    # exactly the same code as before
  end
end
```

正如你所见，我们所做的更改非常小。现在，如果我们在启动 GenServer 时传递 mode: :manual，GenServer 将不会启动 ticking 循环。这意味着我们需要一种手动触发 tick 的方式。这听起来可能很糟糕，但我们认为最好的方式是向 GenServer 发送一个 :tick 消息，以精确模拟 mode: :periodic 的行为。这听起来不好，因为它通过依赖 GenServer 的内部实现细节而破坏了黑盒模型。然而，我们认为这是一种简单且实用的解决方案，能够实现测试的目的：如果我们更改内部实现，测试将会失败，这并不理想，但至少会发出信号表明有问题需要修复，而不是悄无声息地溜过去。我们本可以想出更优雅的方式来隐藏这一实现细节，例如在 GenServer 中公开一个 tick/0 函数，但我们认为将对应用程序代码的更改保持在最小范围所带来的好处大于隐藏这一细节。

现在我们的测试可以在 :manual 模式下启动 GenServer，并使用 Mox.expect/4 来断言 send_sms/2 在测试期间恰好被调用一次：

```
testing_otp/periodic_actions/weather_checker_with_mode_test.exs

defmodule SoggyWaffle.WeatherCheckerTest do
  use ExUnit.Case, async: true

  # same setup as before,

  test "when the process \"ticks\", the Twilio interface is called" do
    interval_in_ms = 5
    phone_number = "+1 111 11 1111"
    test_pid = self()
    ref = make_ref()

    start_options = [
      # The :mode option is set explicitly for testing.
      mode: :manual,
      interval: interval_in_ms,
```
```
city: "Los Angeles",
phone_number: phone_number
]

pid = start_supervised!({SoggyWaffle.WeatherChecker, start_options})

expect(SoggyWaffle.TwilioMock, :send_sms, fn to, text ->
  assert to == phone_number
  # TODO: assert on text
  send(test_pid, {:send_sms_called, ref})
  :ok
end)

send(pid, :tick)

  assert_receive {:send_sms_called, ^ref}
  end
end
```

还有一个最后的问题需要解决：如果我们想在应用程序的监督树中启动我们的 GenServer 怎么办？这种情况相当可能，但这意味着我们的测试无法依赖在测试中启动另一个 SoggyWaffle.WeatherChecker，因为应用程序级别的 GenServer 和测试级别的 GenServer 都将调用同一个测试替身，从而使我们的 Mox.expect/4 断言变得不可靠。

这个问题更具普遍性，与单例资源有关，单例资源是应用程序中作为单例并由应用程序在启动时启动的组件。这些资源往往会在测试中引发不同类型的问题，因为通常你 either 不能在测试中启动这些资源的其它实例（毕竟它们是单例），或者，如果你能在测试中启动其它实例，它们将与单例实例争夺相同的资源。让我们在下一节更深入地探讨单例资源。

## 测试单例资源

在 Elixir 和 Erlang 应用程序中拥有 *单例资源* 非常常见。单例资源在你的应用程序中以单一且唯一的单元存在。例如，具有全局注册名称的进程就是一个单例资源，因为系统中在同一时间不能存在另一个具有相同名称的进程。这个例子有点过于简化了。

更常见且更现实的例子是一个进程执行某项操作，该操作最多应由一个进程在同一时间执行——例如，需要独占访问文件系统上的文件的进程，或周期性地通过短信通知用户即将下雨的进程。你明白我们对后者的意图吗？如果我们有多个这种类型的进程，我们会遇到麻烦，因为每个进程都会尝试发送短信，导致用户被多次通知。解决方案可能是让进程之间共享已通知用户列表，例如在 ETS 表中。然而，我们只是把问题转移了：单例资源现在变成了 ETS 表。哎呀。那么，我们如何测试这些场景呢？

我们真心认为单例通常是测试中最棘手的事情之一。*编写* 单例资源通常并不具有挑战性，因为在任何给定时间你都只有一个资源实例在运行。然而，当你想要 *测试* 这些实例时，你通常希望启动单例资源的额外实例以便在隔离环境中进行测试。我们将介绍一些测试单例资源的变通方法，但不要期望得到关于干净且成熟的测试模式的强烈建议。在这种情况下，测试可能需要逐案考虑，似乎 Elixir 社区尚未找到一种被广泛接受的解决方案。
