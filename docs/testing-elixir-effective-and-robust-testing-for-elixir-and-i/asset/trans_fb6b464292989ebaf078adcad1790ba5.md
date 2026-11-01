
## 完全存根接口

通常我们并不愿意在所有测试中都使用 mock 模块。假设我们有一些测试覆盖了内部调用 rain?/2 的代码片段。在这种情况下，我们可能不想对 SoggyWaffle.WeatherAPIMock 模块中的函数设置期望，因为我们不关心 mock 模块是如何被调用的。我们希望存根 SoggyWaffle.WeatherAPIMock 中的函数，使其做最少的工作来假装与天气 API 的交互一切正常。

然而，在每个最终会调用 get_forecast/1 的测试中都存根所有函数很快就会变得很麻烦。因此，Mox 提供了 Mox.stub_with/2 函数，用另一个模块中的实现来存根 mock 模块中的所有函数。例如，我们可以定义一个 NoOpWeatherAPI 模块，它实现 SoggyWaffle.WeatherAPI.Behaviour behaviour，但在 get_forecast/1 函数中什么都不做。

```
integration_tests/soggy_waffle/no_op_weather_api.ex

defmodule SoggyWaffle.NoOpWeatherAPI do
  @behaviour SoggyWaffle.WeatherAPI.Behaviour

  @spec get_forecast(String.t()) :: {:ok, map()}
  def get_forecast(city) do
    response = %{
      "list" => [
        %{
          "dt" => DateTime.to_unix(DateTime.utc_now()),
          "weather" => [%{"id" => _thunderstorm = 231}]
        }
      ]
    }

    {:ok, response}
  end
end
```

一旦我们有了 SoggyWaffle.NoOpWeatherAPI，我们就可以在不关心断言 SoggyWaffle.WeatherAPIMock 如何被调用的每个测试中，将其用作 SoggyWaffle.WeatherAPIMock 的存根。为此，只需对这些测试调用 stub_with/2 即可：

```
Mox.stub_with(SoggyWaffle.WeatherAPIMock, SoggyWaffle.NoOpWeatherAPI)
```

如果我们希望对所有测试默认使用 no-op 模块，并且只在少数断言接口使用方式的测试中使用 mock 模块，我们可以直接使用应用程序环境。例如，我们可以在测试环境的配置中设置应用程序使用 no-op 模块：

```
import Mix.Config

config :soggy_waffle, :weather_api_module, SoggyWaffle.NoOpWeatherAPI
```

然后，在我们希望使用 mock 来验证期望的测试中，我们可以将 :weather_api_module 的值设置为 SoggyWaffle.WeatherAPIMock：

integration_tests/soggy_waffle/test/soggy_waffle_test_app_env_setup.exs

```
defmodule SoggyWaffleTest do
  use ExUnit.Case

  setup do
    current_weather_api_module =
      Application.fetch_env!(
        :soggy_waffle,
        :weather_api_module,
        SoggyWaffle.WeatherAPI
      )

    Application.put_env(
      :soggy_waffle,
      :weather_api_module,
      SoggyWaffle.WeatherAPIMock
    )

    on_exit(fn ->
      Application.put_env(
        :soggy_waffle,
        :weather_api_module,
        SoggyWaffle.NoOpWeatherAPI
      )
    end)
  end

  # tests
end
```

应用程序环境是一个全局存储；因此，如果我们采用这种方法，就不能使测试用例异步，并且必须记住在测试后将 :weather_api_module 的值在应用程序环境中设置回原始值。

在退一步讨论依赖双重的好处之前，让我们看一个最后的模式来修复我们引入代码中的一个小代码异味。

## 编译时提升性能

我们讨论的模式功能强大且灵活，它们让我们能够精细控制对外部服务的测试交互。然而，我们仍然违背了一个原则：我们在生产代码中引入了运行时调用来决定使用哪个 double。也就是说，每次我们调用 rain?/2 函数时，我们都会调用 Application.get_env/3（本质上是从 ETS 表中读取）来获取我们想要使用的 double 模块。为了适应测试而更改生产代码通常是必要的恶，但应尽可能限制，尤其是在伴随性能成本时。在这种情况下，性能损失相比于生产环境中对 API 的 HTTP 调用成本可以忽略不计。然而，你可以想象出我们可能希望削减 Application.get_env/3 中从 ETS 表读取所需的那几微秒的情况。

为了解决这个问题，一个可能的初始方法是使用 Application.compile_env/3 在编译时读取模块。compile_env/3 的工作方式与 get_env/3 类似，但此函数强制在编译时使用，并允许 Elixir 跟踪在运行时和编译时之间发生变化的配置值。我们在这里不会特别关注此功能，但在此类情况下使用 compile_env/3 是良好的实践。

```
integration_tests/soggy_waffle/soggy_waffle_compile_time_di.ex

defmodule SoggyWaffle do
  @weather_api_module Application.compile_env(
    :soggy_waffle,
    :weather_api_module,
    SoggyWaffle.WeatherAPI
  )

  def rain?(city, datetime) do
    with {:ok, response} <- @weather_api_module.get_forecast(city) do
      weather_data =
        SoggyWaffle.WeatherAPI.ResponseParser.parse_response(response)

      SoggyWaffle.Weather.imminent_rain?(weather_data, datetime)
    end
  end
end
```

这种方法在某些情况下效果很好，但它有一个关键的缺点：你只能在整个测试环境中使用一个 double 模块。原因在于我们在编译时读取应用程序环境，因此我们的代码将使用特定的 double 模块作为 @weather_api_module 的值进行编译。这意味着你将无法同时测试实际的 SoggyWaffle.WeatherAPI 模块并像本章讨论的那样使用 doubles 来隔离测试。

为了解决这个问题，我们设计了一个利用 Mox.stub_with/2 的模式。这个想法可能听起来很 confusing，但这里是： 在测试期间使用 double 作为默认模块，并在需要测试与外部系统的实际交互时用真实模块对其进行存根。基本上，你正在使用一个 double 并用真实模块存根该 double。疯狂吧。让我们实际看看。

我们将继续在编译时从应用程序环境中读取并将天气 API 模块存储在 @weather_api_module 中。在 dev 和 prod 环境中，此模块将是 SoggyWaffle.WeatherAPI，即真实模块。在测试环境中，我们将模块设置为 SoggyWaffle.WeatherAPIMock。然而，当我们想通过与 API 的真实交互来测试某些代码时，我们将调用 stub_with/2：

```
integration_tests/soggy_waffle/test/soggy_waffle_test_stub_with.exs

describe "rain?/2" do
  test "success: using the real API" do
    Mox.stub_with(SoggyWaffle.WeatherAPIMock, SoggyWaffle.WeatherAPI)

    # rest of the test
  end
end
```

在此测试中，天气 API mock 将“路由”函数调用到真实的 SoggyWaffle.WeatherAPI 模块，让我们能够测试它。

这种模式在 dev 和 prod 环境中具有零运行时影响，而这正是性能最重要之处。通过使用这个技巧，你将有效地尽可能接近不更改生产代码以适应测试，同时仍然使用依赖注入并能够隔离测试。

现在我们已经了解了不同种类的依赖双重，让我们讨论双重为系统设计带来的好处。

## 依赖双重的隐藏好处

让我们退一步。我们从外部依赖的角度谈论了集成测试。正如我们在前几节中看到的那样，通常的一个经验法则是，如果我们拥有依赖项，就在测试中直接使用外部依赖项，否则使用依赖双重。

依赖双重是测试的强大工具，但它们的好处也延伸到应用程序的设计。如果我们回顾为能够使用天气 API 接口的 double 所做的更改，有两个关键点：在调用点使接口可交换（在 rain?/2 中）以及为接口定义 behaviour。这两件事最终使我们的应用程序耦合更少、架构更好。现在，我们在系统和外部系统之间有一个清晰的分离点。与外部系统的接口由 behaviour 明确定义，这迫使我们思考如何与依赖项交互。

最后，使外部系统的接口可交换意味着我们可以灵活地将其更改为适合特定环境。例如，我们可能希望在测试中使用 SoggyWaffle.WeatherAPIMock，在开发应用程序并在本地尝试时使用 SoggyWaffle.FakeWeatherAPI，而在验收、暂存或生产环境中使用真实的 SoggyWaffle.WeatherAPI 模块。
