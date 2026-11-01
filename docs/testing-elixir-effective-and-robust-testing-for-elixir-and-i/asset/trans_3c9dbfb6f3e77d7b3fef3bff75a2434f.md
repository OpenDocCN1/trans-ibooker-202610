
一般在测试单例资源时，我们建议你首先确定*是什么使其成为单例资源*。是注册的名称吗？是它被允许操作特定文件的唯一事物这一事实吗？也许它是与外部系统的唯一连接？让我们看看如何找出这一点以及如何进行测试。

## 使单例资源的本质可定制

在之前的章节中，当我们为 SoggyWaffle.WeatherChecker 添加 mode: :manual 选项时，我们违背了我们核心的信念，即不应仅为了使测试更容易而更改应用程序代码。好吧，我们又得打破这一信念了。测试单例资源最简单、最容易且最高效的方法之一就是使使它们成为单例资源的东西变得*可定制*。

由于我们在之前的章节中已经使用过一个单例资源（SoggyWaffle.WeatherChecker），我们将以此作为案例研究。在这种情况下，使 SoggyWaffle.WeatherChecker 成为单例资源的原因是它被允许向用户发送短信消息的唯一事物（为了避免向同一用户发送多条消息）。在代码中，我们可以将这一想法翻译为以下陈述：SoggyWaffle.WeatherChecker 是在任何给定时间被允许使用 SoggyWaffle.Twilio.send_sms/2 的唯一事物。

第 2 章《*集成和端到端测试*》的内容在这里将适用得很好。我们的天气检查器 GenServer 已经在编译时从应用程序环境中读取用于 Twilio 接口的模块。

正如我们在讨论集成测试时提到的，特别是 *Stubbing Entire Interfaces* 中，使用依赖双重的一个有用且灵活的技术是在构成测试套件的大多数测试中使用无操作假对象，但在专门测试我们感兴趣的行为的测试中使用实际的模拟。

在这种情况下，我们可以在除天气检查器 GenServer 的测试之外的所有测试中使用一个不执行任何操作的 Twilio 假对象。假对象的代码简短精炼：

```
testing_otp/periodic_actions/no_op_twilio.ex
```

```elixir
defmodule SoggyWaffle.NoOpTwilio do
  @behaviour SoggyWaffle.Twilio.Behaviour

  @impl true
  def send_sms(_phone_number, _text), do: :ok
end
```

然后，每当我们需要启动一个天气检查器进程而不让它与 Twilio 或双重对象交互时，我们可以使用 Mox.stub_with/2：

```
Mox.stub_with(SoggyWaffle.TwilioMock, SoggyWaffle.NoOpTwilio)
```

我们在应用程序的监督树中启动天气检查器时也必须使用 mode: :manual，以避免天气检查器在后台运行。

在测试我们的 GenServer 时，我们现在可以切换到 SoggyWaffle.TwilioMock 模拟，并测试天气检查器是否对 Twilio 进行了正确的调用。

## 单例问题的更脏解决方案

有时你可能会觉得自己处于一种情况，即你有一个单例资源，根本无法在测试环境中启动。让我们想一个疯狂的例子：你的应用程序需要在每小时的第37分钟强制重启整个 VM，因为这是业务需求。你可以用 cron 作业来实现这一点，但我们说的是疯狂的例子，所以假设你实现了一个 GenServer，每分钟执行一次检查，看看当前分钟是否是小时中的第37分钟。如果是第37分钟，GenServer 就会重启 VM。如何测试这样的东西？

有些人想出的解决方案是在测试环境中使用*不同的应用程序监督树*。例如，你的 Application.start/2 回调可能看起来像下面的这样：

```
testing_otp/singleton_resources/different_sup_tree_for_testing/application.ex
```

```elixir
@mix_env Mix.env()

def start(_type, _args) do
  Supervisor.start_link(children(@mix_env), strategy: :one_for_one)
end
```

如你所见，我们调用 children(@mix_env) 以根据 Mix 环境确定应用程序顶级监督器的子进程。这样，我们就可以为测试环境设置与开发和生产环境不同的子进程集。你问为什么要在编译时读取 Mix.env() 并将其存储在 @mix_env 中？好吧，因为在 Elixir 发布版的运行时，Mix 模块（以及 :mix 应用程序）不可用，[25] 所以我们希望在编译代码时读取 Mix 环境。观察得好！

现在回到我们的代码。children/1 函数可能看起来像这样：

```elixir
defp children(:test) do
  [
    MyApp.OtherChild
  ]
end

defp children(_env) do
  [
    MyApp.VMRestarter,
    MyApp.OtherChild
  ]
end
```

我们只在开发和生产环境中启动 MyApp.VMRestarter GenServer，而不在测试环境中启动。然而，如果我们在测试环境中不启动 GenServer，我们如何测试 GenServer 本身呢？好吧，在这种情况下，你不会测试。无法测试 GenServer 是我们强烈不鼓励这样做的主要原因。

根据我们的经验，没有任何情况是精心放置的依赖双重（假对象、模拟或存根）无法拯救局面的。虽然在上述情况下，你可能不得不稍微偏离正道，对“依赖双重”是什么保持灵活。例如，在这种情况下，GenServer 可能必须调用类似 Erlang 函数 :init.reboot/0 的东西来重启 VM。[26] 好吧，那么你就可以围绕此函数包装一个接口模块并为其创建依赖双重：

```
testing_otp/singleton_resources/different_sup_tree_for_testing/rebooter.ex
```

```
defmodule MyApp.RebooterBehaviour do
  @callback reboot() :: :ok
end

defmodule MyApp.Rebooter do
  @behaviour MyApp.RebooterBehaviour

  @impl true
  defdelegate reboot(), to: :init
end

Mox.defmock(RebooterFake, for: MyApp.Rebooter)
```

在这里，你将在测试中使用 RebooterFake 模块，并在开发和生产中使用 MyApp.Rebooter，就像我们在之前的章节中展示的那样。

我们理解这种做法会感觉多么“不干净”。幸运的是，在我们的职业生涯中，我们并没有遇到太多需要诉诸如此极端措施的情况。不过在那为数不多的情况下，我们发现这种方法是最不糟糕的解决方案，我们更喜欢它而不是不同的监督树解决方案。最不糟糕远非最好，但它比最糟糕要好，对吧？

真相是，代码越纠缠复杂，并且与世界的复杂部分（比如主机，或在谈论周期性操作时*时间本身*）交互越多，就越难以控制这些世界的部分并使测试保持干净。好消息是这些问题有一些变通方法。坏消息是它们并不总是整洁美观。

现在我们已经介绍了如何测试 GenServer、周期性操作和单例资源，让我们继续测试另一个不利于测试的 Elixir 和 Erlang 特性，*弹性*。

也就是说，让我们谈谈测试在进程级别出现问题时会发生什么。

## 测试弹性

让我们用一个大胆的声明开始这一节：我们不是测试进程崩溃的粉丝。我们要来点哲学。为什么进程会崩溃？大多数时候，因为*意外的错误*。在某些情况下，最好的做法可能是为我们知道可能发生的错误条件（*预期的错误*）引发异常或退出进程。然而，我们发现这往往是例外而不是规则，因为如果错误是预期的，那么你可能希望优雅地处理它（想想 TCP 连接中断或用户输入错误）。在你故意引发或退出时，测试该行为可能有意义。

不管怎样，OTP 架构最强大的特性之一是，如果进程遇到意外错误并崩溃，很可能有监督器将其重新启动。这不是我们想要测试的行为；监督器是有效的，并且几十年来一直通过自动化和实地技术进行测试。同时，我们也不真的想测试进程*可以崩溃*。但如果它们因意外错误而崩溃，如果错误本身是意外的，我们如何测试它？如果你可以测试一个意外错误，我们认为该错误按定义就不是意外的。这就是你的哲学。

所以，我们不想测试进程崩溃时是否会被重启，也不想测试进程因意外错误而崩溃。那我们测试什么呢？好吧，关于进程崩溃的一个有趣的测试点是崩溃的*后果*：换句话说，*崩溃恢复*。大多数 OTP 的设计方式是使进程崩溃时大多数事物自动清理，使用进程和端口之间的链接等。例如，如果一个 GenServer 使用 `:gen_tcp` 启动一个 TCP socket，该 socket 将与 GenServer 链接。如果 GenServer 崩溃，socket 将因现有链接而被关闭。
