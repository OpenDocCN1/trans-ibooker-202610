
```
>> Application.get_env(
>> :soggy_waffle,
>> :weather_api_module,
>> SoggyWaffle.WeatherAPI
>> )

with {:ok, response} <- weather_api_module.get_forecast(city) do
  weather_data =
    SoggyWaffle.WeatherAPI.ResponseParser.parse_response(response)

  SoggyWaffle.Weather.imminent_rain?(weather_data, datetime)
end
end
```

第三个参数 Application.get_env/3 是在应用程序环境中未设置 :weather_api_module 键时返回的默认值。这意味着默认情况下行为仍然使用真实的依赖。我们可以在测试环境中仅配置 :weather_api_module 的值，以便仅在测试时使用假接口。配置应用程序环境的一种常见方式是通过 Mix 配置。

# integration_tests/soggy_waffle/config/test.exs

```
import Mix.Config

config :soggy_waffle, :weather_api_module, SoggyWaffle.FakeWeatherAPI
```

由于我们在测试环境中使用 SoggyWaffle.FakeWeatherAPI，因此我们可以修改测试，以便断言 SoggyWaffle.FakeWeatherAPI 记录的输出：

# integration_tests/soggy_waffle/test/soggy_waffle_test.exs

```
defmodule SoggyWaffleTest do
  use ExUnit.Case

  import ExUnit.CaptureLog
```

```
describe "rain?/2" do
  test "success: gets forecasts, returns true for imminent rain" do
    log =
      capture_log(fn ->
        SoggyWaffle.rain?("Los Angeles", DateTime.utc_now())
      end)

    assert log =~ "Getting forecast for city: Los Angeles"
  end
  end
  end
```

现在 SoggyWaffle.WeatherAPI 只实现了一个函数，这可以正常工作。然而，在应用程序开发过程中，我们可能会不断向 SoggyWaffle.WeatherAPI 添加更多函数。我们如何跟踪 SoggyWaffle.FakeWeatherAPI 需要实现的所有函数以镜像 SoggyWaffle.WeatherAPI 呢？让我们在下一节中弄清楚。

## 使用行为与外部依赖交互

SoggyWaffle.WeatherAPI 模块提供的接口很简单，因为它只由一个函数组成。然而，如果我们扩展 SoggyWaffle.WeatherAPI 模块的功能，保持 SoggyWaffle.FakeWeatherAPI 与 SoggyWaffle.WeatherAPI 镜像同步就会变得困难。这种情况非常适合使用行为，行为是 Elixir 模块，定义了一组函数（接口），其他模块可以同意实现。我们可以定义一个行为来指定我们希望如何与天气 API 接口，然后在真实的天气 API 接口以及假接口中都实现该行为。

```
integration_tests/soggy_waffle/weather_api_behaviour.ex

defmodule SoggyWaffle.WeatherAPI.Behaviour do
  @callback get_forecast(city :: String.t()) ::
    {:ok, term()} | {:error, term()}
end
```

现在我们可以将 @behaviour SoggyWaffle.WeatherAPI.Behaviour 行添加到 SoggyWaffle.WeatherAPI 模块以及 SoggyWaffle.FakeWeatherAPI 模块中。为接口提供行为有两个好处。第一个是行为会在编译时被检查；因此，如果我们向 SoggyWaffle.WeatherAPI.Behaviour 添加函数但忘记将其添加到实现该行为的所有模块中，那么我们将收到编译时警告。另一个好处是行为会清晰地定义外部系统的边界：行为代表我们用于与外部系统交互的接口。

行为是处理替身的强大工具，但与手动创建“假”模块相比，处理替身还有更简单的方法。

在下一节中，我们将了解不同类型的测试替身以及如何使用它们。

## 测试替身：存根、模拟和伪造

到目前为止，我们只是使用术语 *external dependency double* 来指代充当外部依赖的东西。然而，我们可以更精确地定义三种替身：存根、模拟和伪造。我们已经用 SoggyWaffle.FakeWeatherAPI 看到了 *fake* 的例子。*fake* 只是在不调用外部依赖的情况下充当外部依赖的东西。*stub* 是一种替身，其函数实现可以根据需要被替换以实现隔离，例如在一次测试中替换函数做一件事，在另一次测试中做另一件事。存根可以根据测试需求为给定函数提供不同的行为。存根在与 *mocks* 结合使用时最有用。模拟本质上是一种存根，可用于断言替身被调用的方式和次数。

与伪造相比，存根和模拟需要多做一些工作，因为我们必须能够处理接口的特定函数，而不是一次性伪造整个接口。幸运的是，在 Elixir 中我们可以使用名为 Mox 的库。[14] Mox 让我们能够基于行为定义测试替身，并提供功能来为行为中的单个函数创建模拟或存根。该库通过按需定义动态模块工作，这些模块具有实现你要为其构建模拟的行为的函数接口。非常酷。

我们要做的第一件事是将 Mox 添加到应用程序的依赖项中。我们仅在测试环境中添加该依赖，因为我们不希望将 Mox 引入生产应用程序：

```
integration_tests/soggy_waffle/mix_with_mox.exs

defp deps do
  [
    # other dependencies
    {:mox, ">= 0.0.0", only: :test}
  ]
end
```

要开始使用 Mox，我们可以使用 Mox.defmock/2 函数。此函数接受模块名称和第二个参数中的 :for 选项。模块名称将是 Mox 将生成的测试替身的名称。在我们的天气 API 示例中，该模块名称可以是 SoggyWaffle.WeatherAPIMock。:for 应该是一个模块（或模块列表），它将作为传递给函数的第一个模块的测试替身。传递给 :for 的模块应该是行为，因为 Mox 就是这样生成提供正确接口的模块的。在我们的天气 API 示例中，我们可以为 SoggyWaffle.WeatherAPI.Behaviour 行为生成一个测试替身。使用 Mox.defmock/2 的一个好地方是应用程序的 test/test_helper.exs，它在 ExUnit 启动测试套件时执行。

```
integration_tests/soggy_waffle/test/test_helper.exs
```

```
Mox.defmock(SoggyWaffle.WeatherAPIMock,
  for: SoggyWaffle.WeatherAPI.Behaviour
)
```

### 应该将模拟定义放在哪里？

入门时，将 Mox.defmock/2 调用放在 test/test_helper.exs 中。Mix 会让 Mox 在运行测试套件之前动态定义模拟。

然而，你可能会发现以这种方式在运行时定义模拟会导致一些编译警告，如果你使用本书中的某些技术，例如在应用程序代码中在编译时读取依赖替身模块。如果是这种情况，你也可以选择在编译时通过 Mox 定义模拟模块。这样，模拟模块将在运行任何测试套件之前像应用程序中的其他模块一样被编译。

为此，我们建议你创建一个类似 test/support/mocks.ex 的文件，并将所有的 Mox.defmock/2 调用放在其中。由于该文件不在 lib 目录内，Elixir 不会自动编译它，因此我们需要调整 mix.exs。你可以通过在 mix.exs 文件的 project/0 函数中 :elixirc_paths 选项下列出“编译路径”来告诉 Elixir 你想要编译的文件在哪里。通常，你希望根据 Mix 环境选择编译路径，以便模拟仅在测试期间编译和加载，而不会包含在生产代码中。

```
def project do
  [
    # other options,
    elixirc_paths: elixirc_paths(Mix.env())
  ]
end

defp elixirc_paths(:test), do: ["lib", "test/support"]
defp elixirc_paths(_env), do: ["lib"]
```

Mox 提供了两个主要函数来创建行为中函数的存根和模拟。第一个称为 Mox.stub/3。此函数接受模拟模块、要存根的函数名称以及与要存根的函数具有相同元数的匿名函数。当在模拟模块上调用被存根的函数时，将执行此匿名函数。通常，stub/3 用于单个测试内部。我们可以重写天气 API 示例的测试，以便不使用 SoggyWaffle.FakeWeatherAPI，而是使用一个匿名函数，该函数执行与 SoggyWaffle.FakeWeatherAPI.get_forecast/1 相同的操作：

```
integration_tests/soggy_waffle/test/soggy_waffle_test_with_mox_stub.exs
```

```
defmodule SoggyWaffleTest do
  use ExUnit.Case

  import ExUnit.CaptureLog
  import Mox

  require Logger

  describe "rain?/2" do
    test "success: gets forecasts, returns true for imminent rain" do
```
