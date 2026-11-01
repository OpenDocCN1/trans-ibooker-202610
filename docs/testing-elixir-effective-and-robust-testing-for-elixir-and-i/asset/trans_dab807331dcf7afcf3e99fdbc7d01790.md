
尽可能让协调其他函数响应的部分对内部逻辑保持轻量。除了良好的软件设计之外，测试一个协调多种逻辑的函数即使它本身的逻辑不多也是一项挑战。如果在内部逻辑之前条件会发生变化，你就必须编写测试来设置每种条件，以确保代码被测试正确覆盖。这一场景的方方面面都指向向纯函数重构，正如我们对 SoggyWaffle.WeatherAPI.ResponseParser 所做的那样。

唉，有很多时候我们无法明确划出可以提取到新函数中的边界。在这种情况下，我们仍然有另一种工具可以用来创建受控环境以运行我们的代码。我们可以为依赖项创建一个替代品，从而隔离我们的代码。接下来让我们来看一下这一点。

## 隔离代码

我们可以采用多种策略来测试代码，以消除外部变量，控制被测代码必须执行的情境，并允许我们期望特定的结果。我们将介绍最简单的方法——注入依赖项并创建基本替代品（测试替身），以便为你的测试工具箱增加另一个选项。

## 依赖注入

在利用依赖注入来隔离被测代码行为之前，让我们花点时间定义一下依赖注入。依赖项是你的代码所依赖的任何代码。依赖注入（通常缩写为 DI）是对任何允许你的代码在不硬编码依赖项名称的情况下使用依赖项的系统的花哨称呼，从而允许满足契约的任何代码单元被使用。在 Elixir 中，我们有两种常见的注入依赖项的方式：作为函数的参数以及通过应用程序环境。在测试中使用 DI 允许我们创建行为可预测的替代依赖项，从而使测试能够专注于被测代码内部的逻辑。

将依赖项作为参数传递是在单元测试中注入依赖项的更常见方式，这也是我们首先要关注的风格。在后面的章节中，我们将介绍通过应用程序环境进行依赖注入，以及 Mox，这是一个有助于创建测试替身的工具。

## 测试替身术语

## 测试替身术语

![](img/c08a369b9a1934f7c9c3b485727eca8e_62_0.png)

本章中，我们将把用于测试的生产代码的替代品称为“测试替身”。这涵盖了可以返回预定结果的代码，甚至可以断言它是否被调用，无论是否带特定参数。我们将在下一章深入探讨不同类型的替身。

## 将依赖项作为参数传递

将依赖项作为参数传递就像听起来一样简单，通常是最简单的解决方案。我们可以选择传递函数或模块作为参数。当你像以下代码所示注入模块时，它必须满足隐式契约。否则调用代码将引发异常。SoggyWaffle 模块已编写为允许传入天气预报函数：

```
unit_tests/soggy_waffle/lib/soggy_waffle.ex

1: defmodule SoggyWaffle do
  - alias SoggyWaffle.WeatherAPI
  - def rain?(city, datetime, weather_fn \\ &WeatherAPI.get_forecast/1) do
  -   with {:ok, response} <- weather_fn.(city) do
5:     {:ok, weather_data} =
      SoggyWaffle.WeatherAPI.ResponseParser.parse_response(response)
      SoggyWaffle.Weather.imminent_rain?(weather_data, datetime)
  -   end
  end
10:
  - end
```

这里，在第 3 行，我们将函数引用作为注入依赖项的默认参数提供。这种模式允许我们在执行期间不必传入真实依赖项，同时在测试期间允许传入替身。注入函数是在你有一个复杂的依赖项，即使在相同的测试设置中也可能返回不一致的结果时可以采用的工具。注入替身允许你消除未知因素，并为被测代码提供一致的测试环境。我们将在下一章探讨的注入整个模块，在你的代码将调用该模块的多个函数时可能很有用。

在我们的代码中，另外两个依赖项 SoggyWaffle.WeatherAPI.ResponseParser 和 SoggyWaffle.Weather 都是纯函数式的，并且各自经过了良好的测试。因此，我们可以通过不担心为这些依赖项注入替身来节省额外的工作。它们永远不会产生不同的结果。我们需要在测试场景中移除的唯一部分——这就是我们称之为代码隔离的原因——是第 4 行对天气 API 的调用。它很容易根据代码执行的时间返回不同的结果，从而阻止我们的测试期望精确的结果。

现在我们添加了注入依赖项的机制，让我们编写一个可以利用该机制创建结果始终相同的场景的测试。将以下测试文件（test/soggy_waffle_test.exs）添加到你的应用程序中。

```
unit_tests/soggy_waffle_examples/test/soggy_waffle_test.exs

1: defmodule SoggyWaffleTest do
-   use ExUnit.Case
- 
-   describe "rain?/2" do
5:   test "success: gets forecasts, returns true for imminent rain" do
-   now = DateTime.utc_now()
-   future_unix = DateTime.to_unix(now) + 1
-   expected_city = Enum.random(["Denver", "Los Angeles", "New York"])
```

```
- test_pid = self()
10:
- weather_fn_double = fn city ->
- send(test_pid, {:get_forecast_called, city})
- data = [
%{
15:
- "dt" => future_unix,
- "weather" => [%{"id" => _drizzle_id = 300}]
- }
- ]
20:
- {:ok, %{"list" => data}}
- end
- assert SoggyWaffle.rain?(expected_city, now, weather_fn_double)
25:
- assert_received {:get_forecast_called, ^expected_city},
- "get_forecast/1 was never called"
- end
- end
end
30:
```

此测试的主要特点是在第 11 行定义并使用函数式测试替身 weather_fn_double。它符合 SoggyWaffle.WeatherAPI.get_forecast/1 的契约，但与真实函数不同，它总是返回完全相同的结果。现在我们的测试可以断言特定值，帮助我们知道被测模块 SoggyWaffle 的行为是否正确。

不过请注意，测试替身不仅仅满足契约，它还有一种机制来报告函数已被调用，并将传递给它的参数发送回测试，见第 12 行。当替身被调用时，模块会向测试进程发送 {:get_forecast_called, city} 消息。

第 26 行的测试代码确保函数已被调用，同时也确保传递给它的值是正确的。我们称这种测试风格为期望。这样，如果该依赖项从未被调用，测试就会失败。这种期望在代码有预期副作用且测试关注确保其发生时最有用。坦白说我们的测试只是通过函数调用进行查询，所以在这种情况下期望并不是完全必要的，但它仍然很好地说明了如何向测试替身添加期望。

在继续之前，值得注意此测试的其他一些特点。被测代码在其业务逻辑中使用了时间比较，实际上位于一个函数式依赖项 SoggyWaffle.Weather 中。因为我们没有将 SoggyWaffle 与该依赖项隔离，关于时间比较的知识现在已渗透到测试 SoggyWaffle 所需的知识中。这与我们之前关于为测试定义单元或黑盒的讨论相关。因为 SoggyWaffle.Weather 已得到充分测试，采用这种方法几乎没有缺点，因为从长远来看它会更容易理解和维护，而这是在我们设计测试时非常重要的优先事项。

匿名函数在第 11 行的测试内部定义，而不是在同一文件中定义命名函数，因为它需要访问绑定到 future_unix 的值。此外，我们必须在函数外部定义测试的进程 ID（或 PID），因为 self() 不会在函数执行时才被求值，而是在被测代码的进程内部执行，该进程可能与我们的测试具有不同的 PID。因为匿名函数在 Elixir 中是闭包，所以 future_unix 的值在函数执行时是函数的一部分。对 self() 的调用
