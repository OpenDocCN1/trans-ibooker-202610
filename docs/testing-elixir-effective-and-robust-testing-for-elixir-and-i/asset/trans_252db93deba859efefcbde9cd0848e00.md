
将单个函数作为单元对待。保持范围狭窄的优势在于，如果被测代码存在问题，保持范围小有助于你快速确定问题所在。狭窄范围的缺点是，通常为了隔离被测代码，你需要在测试中编写更多代码，这可能会使难以跟上正在发生的事情。允许你的测试范围（黑盒）包含一个经过良好测试的功能性依赖，可以让你在利用狭窄范围的同时，避免隔离使用该依赖的代码的一些步骤。

你的测试需要以两种方式与被测代码交互。第一种更明显：你的测试会用参数调用一个函数（或发送消息）并获得返回值。第二种更复杂。你的测试可能需要拦截外部调用并返回响应，代替代依赖，如果你的代码依赖于代码库中的其他代码。我们称之为“隔离被测代码”。我们将在本章后面介绍代码隔离，但首先我们将看看在何时可以不用严格隔离。以下图表显示了围绕被测代码绘制的黑盒。

![](img/c08a369b9a1934f7c9c3b485727eca8e_26_0.png)

由于被测代码依赖于代码库中的其他代码，测试必须解决代码隔离问题。存在不同的代码隔离方法，但它们都伴随着成本，通常体现在时间和测试复杂性上。纯函数式代码的存在会影响需要隔离的内容。一个函数是“纯”的，如果它在每次使用相同参数调用时都返回完全相同的答案并且没有副作用。我们将在本章后面进一步展开。当你的依赖是纯函数式的并且本身经过了良好的测试时，你可以扩展黑盒以包含该依赖，如图所示。

![](img/c08a369b9a1934f7c9c3b485727eca8e_27_0.png)

测试范围

扩展黑盒使我们能够跳过隔离代码的额外工作，同时不放弃轻松定位损坏代码的能力。你的测试仍然紧密聚焦于被测代码，并且——由于你的代码是纯函数式的——你在其中测试代码的环境是完全受控的，防止了不一致的结果。当你能够以这种方式包含依赖项时，你的测试更容易编写、理解和维护。由于你的依赖项本身已被测试，你可以编写更少的测试代码，同时在测试失败时仍能确定代码损坏的位置。如果你的依赖项的测试没有失败，但在你引入它的测试中出现失败，你就知道你的失败是在新代码中，而不是在依赖项中。

挑战在于找到何时应在测试范围中包含纯函数式依赖项的平衡点。调整这种感觉需要一些试错和从痛点中学习。首先扩展黑盒以包含这些依赖项，然后如果发现调试代码时出现问题，就收紧范围。实践将帮助你磨练何时这是正确选择的判断力。

现在，让我们学习编写一些测试。

## 使用 ExUnit 进行测试

从一开始，Elixir 就是在核心库中将 ExUnit 测试框架作为一部分进行开发的。因此，我们在生产应用程序中使用的大多数测试工具都直接来自 Elixir 核心。这意味着，如果你拥有 Elixir 并且熟悉可用的工具，你就可以在无需引入其他库的情况下编写有效的测试。

为了学习如何使用 ExUnit，我们将从编写第一个测试开始，然后讨论一些测试理论，特别是测试阶段。我们将查看如何组织测试文件，然后探索使用常见的 Elixir 来最大化测试的有效性。最后，我们将介绍如何设计代码，使其组织良好且易于测试。

## 我们的第一个测试

让我们在我们的降雨警报应用 Soggy Waffle 中编写第一个测试。Soggy Waffle 会调用 API 并从中获取数据。然后它将数据标准化为自己的术语和数据结构，供应用程序其余部分使用。虽然本章不会重点关注对 API 的这些调用（但将在第 2 章 集成和端到端测试中介绍），响应转换是可以在隔离中测试的代码的绝佳示例。让我们看看我们将要测试的代码。

我们在本书的代码中包含了一个 Soggy Waffle 的副本。本章中我们展示的大多数应用程序文件都来自该代码。Soggy Waffle 中的示例测试文件扩展名为 .bak.exs，因此在运行测试时它们不会运行，但它们可供参考。如果你想跟随示例一起编码，你应该在提供的 Soggy Waffle 应用程序内部进行，在示例之前的指示位置添加测试文件。

从理论上讲，只要天气 API 返回时间戳和该时间戳的天气状况，Soggy Waffle 就可以与任何天气 API 配合使用。作为最佳实践，我们的应用程序将 API 响应翻译成 SoggyWaffle.Weather 结构体列表，该结构体在 lib/soggy_waffle/weather.ex 中定义，具有两个字段，一个用于时间戳 datetime，一个用于天气状况是否转化为降雨。

这意味着我们的 ResponseParser 模块将包含大量特定于我们选择的 API 的信息；在这种情况下，那就是 openweathermap.org。请注意代码中的模块属性以及关于该信息来源的注释。

```
unit_tests/soggy_waffle/lib/soggy_waffle/weather_api/response_parser.ex

defmodule SoggyWaffle.WeatherAPI.ResponseParser do
  alias SoggyWaffle.Weather

  @thunderstorm_ids [200, 201, 202, 210, 211, 212, 221, 230, 231, 232]
  @drizzle_ids [300, 301, 302, 310, 311, 312, 313, 314, 321]
  @rain_ids [500, 501, 502, 503, 504, 511, 520, 521, 522, 531]
  @all_rain_ids @thunderstorm_ids ++ @drizzle_ids ++ @rain_ids

  @spec parse_response(Weather.t()) ::
    {:ok, list(Weather.t())} | {:error, atom()}
  def parse_response(response) do
    results = response["list"]

    Enum.reduce_while(results, {:ok, []}, fn
      %{"dt" => datetime, "weather" => [%{"id" => condition_id}]},
      {:ok, weather_list} ->
        # possible weather codes: https://openweathermap.org/weather-conditions

        new_weather = %Weather{
          datetime: DateTime.from_unix!(datetime),
          rain?: condition_id in @all_rain_ids
        }

        {:cont, {:ok, [new_weather | weather_list]}}
      _anything_else, _acc ->
        {:halt, {:error, :response_format_invalid}}
    end)
  end
end
```

我们的测试需要传入一个形似 API 数据的响应，并且它期望响应数据以 SoggyWaffle.Weather 结构体列表的形式存在。

现在让我们在提供的 Soggy Waffle 代码中打开一个新的测试文件 test/soggy_waffle/weather_api/response_parser_test.exs 并开始编写第一个测试！将下面的代码复制到你的文件中。

```
unit_tests/soggy_waffle_examples/weather_api/response_parser_test.exs

1: defmodule SoggyWaffle.WeatherAPI.ResponseParserTest do
-   use ExUnit.Case
-   alias SoggyWaffle.WeatherAPI.ResponseParser
-   alias SoggyWaffle.Weather
5:
-   describe "parse_response/1" do
-   test "success: accepts a valid payload, returns a list of structs" do
-     api_response = %{
-       "list" => [
-         %{"dt" => 1_574_359_200, "weather" => [%{"id" => 600}]},
10:
-         %{"dt" => 1_574_359_900, "weather" => [%{"id" => 299}]}
-       ]
-     }
-
-     assert {:ok, parsed_response} =
15:
-         ResponseParser.parse_response(api_response)
-
-     for weather_record <- parsed_response do
-       assert match?(
-         %Weather{datetime: %DateTime{}, rain?: _rain},
