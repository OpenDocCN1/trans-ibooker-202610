# 附录 1
## 何时随机化测试数据

本书中的许多测试示例展示了随机测试数据的使用。这是一个经验丰富的软件开发人员意见不一的话题。我们添加本附录是为了帮助你理解何时可以安全地使用随机数据，并提供足够的背景信息，帮助你决定是否采用这种方法。

在为测试设置数据时，值可以分为两类：必要数据和偶然数据。如果从偶然数据开始解释，区分两者会更容易。如果数据不应对被测代码的行为产生任何影响，则该数据是偶然的。在第 1 章《单元测试》中，我们使用了以下示例：

```
when_to_randomize_data/soggy_waffle.ex

1: defmodule SoggyWaffle do
  - alias SoggyWaffle.WeatherAPI
  - 
  - def rain?(city, datetime, weather_fn \\ &WeatherAPI.get_forecast/1) do
5:   with {:ok, response} <- weather_fn.(city) do
  -   {:ok, weather_data} =
  -     SoggyWaffle.WeatherAPI.ResponseParser.parse_response(response)
  - 
  -   SoggyWaffle.Weather.imminent_rain?(weather_data, datetime)
  end
10:
  - end
  - end
```

作为提醒，在此示例中，我们的测试传入了一个测试替身。被测代码的行为主要是将接收到的数据传递给其他函数。它第一次这样做是在第 5 行，在那里它将提供的城市名称传递给它同样提供的测试替身。在这种情况下，绑定到 city 变量的值不会以任何方式影响代码的行为。事实上，这里行为的重要部分是，无论绑定到该变量的值是什么，它始终会将其传递给测试替身。

让我们查看测试，以便更好地理解所有这些是如何协同工作的。

```
when_to_randomize_data/soggy_waffle_test.exs
1:  test "success: gets forecasts, returns true for imminent rain" do
    now = DateTime.utc_now()
    future_unix = DateTime.to_unix(now) + 1
    expected_city = Enum.random(["Denver", "Los Angeles", "New York"])
5:  test_pid = self()

    weather_fn_double = fn city ->
    send(test_pid, {:get_forecast_called, city})
    # build return data
    {:ok, %{"list" => data}}
10:
    end

    assert SoggyWaffle.rain?(expected_city, now, weather_fn_double)

    assert_received(
15:
    {:get_forecast_called, ^expected_city},
    "get_forecast was never called"
    )
    end
end
20:
```

当测试替身被调用时执行的第一个代码行是 send(test_pid, {:get_forecast_called, city})（见第 8 行）。这段代码会将绑定到该变量的值发送回测试进程本身。在测试结束时，第 16 行的代码使用 assert_received 来验证被测代码传递给测试替身的值与测试在执行代码时最初发送的值相同。

我们的测试使用了一种简单的方法来随机化数据，使用 Enum.random/1 来选择传入的值以及期望传递给替身的值。在具有更完整数据生成器的应用程序中，例如 Factory.city_name/0 或类似的东西，数据集可能更大，但效果相同：城市名称本身是偶然的，但被测代码必须沿用相同的值。

在此测试中随机化偶然数据可确保被测代码不会更改该数据。我们的测试应关注被测代码的外部契约（保持黑盒），在这种情况下，这意味着输入数据以及发送到另一个函数或进程的数据。

理解偶然测试数据将有助于我们定义必要测试数据。在之前的示例中，我们不需要测试所有可能的输入值。如果特定的一组城市名称有效而其他名称无效，则数据不再被视为偶然，而是必要。我们需要测试每个有效的城市名称是否都有效，而不在该列表中的名称则无效。

书中对此的一个例子来自第 1 章《单元测试》，我们在那里使用列表推导式对我们所知道的天气 API 的每个有效响应值进行断言：

```
when_to_randomize_data/response_parser_test.exs
```

```
defmodule SoggyWaffle.WeatherAPI.ResponseParserTest do
  use ExUnit.Case
  alias SoggyWaffle.WeatherAPI.ResponseParser
```

```
### weather codes come from: https://openweathermap.org/weather-condition
@thunderstorm_ids {"thunderstorm", [200, 201, 202, 210, 211, 212, 221, 230, 231, 232]}
@drizzle_ids {"drizzle", [300, 301, 302, 310, 311, 312, 313, 314, 321]}
@rain_ids {"rain", [500, 501, 502, 503, 504, 511, 520, 521, 522, 531]}

for {condition, ids} <- [@thunderstorm_ids, @drizzle_ids, @rain_ids] do
  test "success: recognizes #{condition} as a rainy condition" do
    now_unix = DateTime.utc_now() |> DateTime.to_unix()

    for id <- unquote(ids) do
      record = %{"dt" => now_unix, "weather" => [%{"id" => id}]}

      assert {:ok, [weather_struct]} =
        ResponseParser.parse_response(%{"list" => [record]})

      assert weather_struct.rain? == true
    end
  end
end
end
```

被测代码 ResponseParser.parse_response/1 的行为会根据传入的响应而不同。如果要防止回归，每个有效的天气状况 ID 都必须进行测试。在测试中，这些 ID 是必要数据。

这可能看起来像是通向基于属性的测试的引子，但了解区别很重要。当你能知道所有有效值时，可以明确地对它们进行测试，将其视为必要数据。当你无法知道所有值但可以定义它们的规则时，可以考虑基于属性的测试。

我们承诺赋予你做出决定的能力，是否应随机化你的偶然测试数据。首先，我们将讨论这样做的理由，然后再介绍通常提出的反对意见。

随机化你的偶然数据有几个好处。如果你正在阅读现有测试并看到数据被随机化，这可以非常快速地表明该数据是偶然的。这将帮助你仅从测试本身推断被测代码的行为。

如果你在编写测试之前编写代码，完全可能不小心出现硬编码值。通常你的测试会帮助你发现这个错误；但如果你刚写完代码，那个硬编码值可能还记忆犹新，你可能会在测试中使用相同的值。这会导致对被测代码行为正确的虚假安全感，尽管它实际上已经严重损坏。这又是随机化数据会有帮助的情况。

随机化你的偶然测试数据也存在潜在风险。许多反对随机化的人经常引用随机值实际导致难以复现的问题的例子。我们见过的一个例子是测试中为时间戳选择随机时区，而被测代码在时区之间转换。结果发现，我们的测试偶尔会选择一个与 UTC 相差半小时的时区。被测代码无法处理不是以整小时偏移的情况。鉴于应用程序只需要提供以整小时偏移的时区，可以认为随机失败是测试的问题而不是代码的问题。

问题在于，理解测试为何首先失败需要一些调试时间。幸运的是，ExUnit 提供了将随机化种子传递给测试运行的能力。如果失败的测试运行输出中报告的种子是 654321，你可以通过在运行 `mix test` 时传递标志 `--seed 654321` 来复现测试运行。结合一些额外的测试输出（`IO.inspect/1` 调用），我们能够识别出问题。但如果我们一开始就用已知的有效值编写测试，我们就不会在这上面浪费时间，而且我们的代码仍然会按预期运行。

请记住，如果你有意尝试找到会破坏代码的值，基于属性的测试是正确的解决方案。随机化测试输入可以发现代码中的特定值依赖，但它无助于保证代码在单次测试运行中对各种输入都有效。

拥有良好的测试失败输出并能够传递随机种子可以帮助减少此类问题，但你的测试运行越异步，就越可能无法完全复现之前的测试运行。

是否偏好随机化你的偶然数据由你决定。但无论如何，理解必要数据和偶然数据之间的区别将有助于你以深思熟虑和有意的方式设计测试。


