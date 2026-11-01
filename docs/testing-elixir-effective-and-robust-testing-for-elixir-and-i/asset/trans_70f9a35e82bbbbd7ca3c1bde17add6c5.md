
在我们的例子中，我们知道多个测试很可能会使用这些 ID 列表，因此我们将它们作为模块属性添加到测试文件的顶部。

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs

@thunderstorm_ids {
  "thunderstorm",
  [200, 201, 202, 210, 211, 212, 221, 230, 231, 232]
}
@drizzle_ids {"drizzle", [300, 301, 302, 310, 311, 312, 313, 314, 321]}
@rain_ids {"rain", [500, 501, 502, 503, 504, 511, 520, 521, 522, 531]}
```

## 利用列表推导式

准备好迎接一些疯狂的操作了吗？我们将把新的测试包装在列表推导式中。将以下代码添加到你的测试文件中，我们将逐步讲解它。保持第一个 for 与之前的测试处于相同的缩进级别。

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs

1: for {condition, ids} <- [@thunderstorm_ids, @drizzle_ids, @rain_ids] do
  - test "success: recognizes #{condition} as a rainy condition" do
  - now_unix = DateTime.utc_now() |> DateTime.to_unix()
  -
5: for id <- unquote(ids) do
  - record = %{"dt" => now_unix, "weather" => [%{"id" => id}]}
  -
  - assert {:ok, [weather_struct]} =
  - ResponseParser.parse_response(%{"list" => [record]})

10:
  - assert weather_struct.rain? == true
  - end
  - end
  - end
```

将代码添加进去后，运行 mix test 来确保测试通过。

在谈论列表推导式之前，让我们先看看在练习步骤中我们传递给函数的那些数据。它是一个手写的 map。现在我们已经通过传递真实响应测试了代码能够处理正确形状的数据，我们就可以相信如果传入正确形状的数据，代码就会正常工作。有效载荷已经被提炼为我们代码关心的部分：一个带有 “dt” 和 “weather” 键的 map，其中 ID 嵌套在 “weather” 键下的 map 列表中。保持输入有效载荷如此之小有助于使我们的测试更容易理解。此外，由于我们是在测试文件内部定义这些值（并且在本例中是在测试内部），因此我们可以安全地对特定值进行断言，而不必担心难以维护的测试。

列表推导式实际上添加了三个新的测试，每个我们感兴趣的天气条件各一个：雷暴、毛毛雨和雨。列表推导式在你有几乎相同的测试但希望错误被分开时很有用。你会看到我们不得不使用 unquote,[11] 一个元编程宏，以便在测试内部访问信息。这可能特别令人困惑，因为它不在 quote 块内，但它有效，并且这是本书中我们将引入的唯一元编程部分。你需要有一种方法为每次迭代提供唯一的测试名称，就像我们在测试名称中使用 condition 的方式一样。如果这些测试中的任何一个失败，ExUnit 的输出会告诉你是哪一个测试，从而更容易追踪问题。

> » 1) test parse_response/1 success: recognizes drizzle as a rainy condition
> (SoggyWaffle.WeatherAPI.ResponseParserTest)
> test/soggy_waffle/weather_api/response_parser_test.exs:32

## 编写自定义测试失败消息

虽然列表推导式允许我们用更少的测试代码覆盖更多测试用例，但它们会带来混淆错误的风险。如果测试失败但你无法判断它是在对哪些值进行断言，你将花费时间调试失败。在我们刚刚编写的测试中，如果雨的值错误，我们如何知道正在测试哪个 ID？让我们更新该断言以使用自定义失败消息：

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs

1: for {condition, ids} <- [@thunderstorm_ids, @drizzle_ids, @rain_ids] do
2:   test "success: recognizes #{condition} as a rainy condition" do
3:     # test body
4:
5:     assert weather_struct.rain? == true,
6:       "Expected weather id (#{id}) to be a rain condition"
7:   end
8: end
```

添加带有字符串插值的自定义错误消息（第 6 行）现在在测试失败时给了我们所需的一切信息。拥有这些信息非常重要，因为否则将无法看到导致测试失败的具体代码。在设计测试时，花时间确保失败消息能直接指向问题所在。它花费不了多久，你或你的团队成员以后会感谢你的。

+   1) test parse_response/1 success:
+      recognizes drizzle as a rainy condition
+      (SoggyWaffle.WeatherAPI.ResponseParserTest)
+      test/soggy_waffle/weather_api/response_parser_test.exs:32
+      » Expected weather id (300) to be a rain condition
+      code: for id <- unquote(ids) do

## 覆盖所有用例

虽然我们将在后面的章节讨论基于属性的测试，但我们通常不需要超出 ExUnit 和 Elixir 之外的东西来覆盖其余用例。我们已经添加了测试来检查返回雨为 true 的条件，但现在我们需要一个测试来确保没有其他代码会产生 true 值。将以下代码添加到你的文件中：

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs
```

```
1: test "success: returns rain?: false for any other id codes" do
  {_, thunderstorm_ids} = @thunderstorm_ids
  {_, drizzle_ids} = @drizzle_ids
  {_, rain_ids} = @rain_ids
5:  all_rain_ids = thunderstorm_ids ++ drizzle_ids ++ rain_ids
  now_unix = DateTime.utc_now() |> DateTime.to_unix()

  for id <- 100..900, id not in all_rain_ids do
    record = %{"dt" => now_unix, "weather" => [%{"id" => id}]}

10:
    assert {:ok, [weather_struct]} =
        ResponseParser.parse_response(%{"list" => [record]})

    assert weather_struct.rain? == false,
        "Expected weather id (#{id}) to NOT be a rain condition."

15:
    end
  end
```

是时候再进行一次测试运行（mix test）来确保你的测试为绿色。定期运行测试总是一个好方法，可以避免在意识到有问题之前走得太远。

这些测试非常彻底，因为它们测试了所有可能的正值。我们能够做到这一点是因为我们有一个已知且有限的降雨天气 ID 列表。我们没有同样全面的非降雨 ID 列表。因此，查看 API 的文档，我们将可能返回的代码范围缩小到 100 到 900 之间。我们再次使用列表推导式，但这次我们使用列表推导式的过滤函数，即时构建 100 到 900 之间所有 ID 的列表 *except* 我们已知为降雨 ID 的那些（第 8 行）。即使这意味着我们测试了很多值，由于我们的代码是纯函数式的，测试仍然会相当快速地运行。我们再次为自己提供了一个自定义错误消息，以确保如果此测试失败，我们知道导致失败的 ID。

此测试本可以使用我们谈到的两种列表推导式方式之一（为每个值创建新的测试或为每个值创建新的断言），两者都有权衡。生成新的测试将消除对自定义错误消息的需求，但会增加实际运行的测试数量。当你决定列表推导式是合适的工具时，由你来决定哪种方式最适合你。

## 测试错误用例

我们编写的每个测试都专注于成功用例。我们在测试名称开头添加 “success” 和 “error” 以帮助在心理上对它们进行排序，但这是一种风格选择，是对无法在测试中嵌套 `describe` 块的回应。现在让我们看看我们的代码可能不会返回成功响应的地方，在本例中是由于输入数据错误。由于我们希望在单独的测试中测试不同的功能，让我们选择格式错误的天气数据。

我们的应用程序相当小，并不打算用于面向客户的生产环境。如果是这样，我们会有更多测试并会利用 Ecto Schema 和 Changeset 函数。我们将在第 4 章 *Testing Ecto Schemas* 中介绍示例的例子。现在，我们将添加一些基本覆盖，前提是当你完成本书后，你将拥有更好的工具来为有效载荷编写更好的验证测试。在同一个 `describe` 块内添加一个新的测试；但这次它是一个 “error” 测试：
