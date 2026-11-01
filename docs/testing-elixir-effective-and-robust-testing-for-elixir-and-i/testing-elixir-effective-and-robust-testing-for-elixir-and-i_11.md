# Appendix 3

## Test Coverage

编写测试时，能够 *测量* 你测试的影响非常有用。你希望知道自己编写的测试正在覆盖你的代码。此外，你希望代码库的“盲点”尽可能少，即在运行测试时从未被执行的代码。代码可能由不同级别的测试运行：单元测试、集成测试和端到端测试。通常，代码库的许多部分会被多个级别的测试覆盖，因为较高级别的测试（如端到端测试）覆盖代码库的大部分内容是很常见的。

衡量测试覆盖了多少代码的最常用指标是 *测试覆盖率*，这是本附录的主题。它是一个 *百分比*，衡量代码库中由测试运行的代码的相对数量。例如，60% 的测试覆盖率意味着每十行代码中有六行在测试套件期间被执行 *至少一次*，但有四行根本未被执行。测试覆盖率支持通常是语言测试工具的一部分，要么作为内置功能，要么作为附加工具。测量覆盖率是一个特定于语言的问题，正如你所想象的，需要深度语言集成才能找出在测试套件期间执行了哪些源代码行。

Elixir 和 Erlang 提供了内置的测试覆盖率支持。Mix 和 ExUnit 随带测试覆盖率功能，提供默认的内置测试覆盖率工具以及对外部测试覆盖率工具的支持。在本附录中，我们将讨论默认的测试覆盖率工具以及与 Coveralls 集成的第三方工具。[56] 我们将在附录结尾探讨特定于 Elixir 的测试覆盖率工具的不足之处。

### Coveralls and the Excoveralls Library

内置测试覆盖率工具开箱即用提供了一组有用的功能：百分比摘要和逐行 HTML 可视化报告。然而，我们中的许多人可能想要一个更“完整”的套件，具有额外的功能，例如这些 HTML 报告的托管、CI 友好的集成，以及对给定项目测试覆盖率历史的洞察。

提供此类功能的常见服务是 Coveralls。[57] 该服务允许你将测试覆盖率结果“发布”到其平台，然后浏览项目、查看洞察、逐文件和逐行浏览报告等。该服务对开源项目免费，并为企业提供付费选项。

Excoveralls 库将 Elixir 项目与 Coveralls 集成，并允许将测试覆盖率报告给 Coveralls。[58] 为了尝试一下，让我们将其作为依赖项添加到 mix.exs 文件中，并通过 :test_coverage 下的 :tool 选项将其设为测试覆盖率工具：

```
defmodule SoggyWaffle.MixProject do
  use Mix.Project

  def project do
    [
      app: :soggy_waffle,
      version: "0.1.0",
      elixir: "~> 1.9",
      start_permanent: Mix.env() == :prod,
      deps: deps(),
      test_coverage: [tool: [ExCoveralls]],
    ]
    end

    defp deps do
    [
{:excoveralls, "~> 0.13.0", only: :test}
  rest of the dependencies
]
end

rest of mix.exs contents
```

现在，运行 `mix test --cover` 将使用 Excoveralls 作为其“覆盖率工具”，而不是内置工具：

```
mix test --cover
Compiling 5 files (.ex)
Generated soggy_waffle app
................

Finished in 1.5 seconds
15 tests, 0 failures

Randomized with seed 131302
------------
COV  FILE                     LINES RELEVANT MISSED
100.0% lib/soggy_waffle.ex          11      3    0
100.0% lib/soggy_waffle/weather.ex      24      5    0
100.0% lib/soggy_waffle/weather_api.ex      20      4    0
100.0% lib/soggy_waffle/weather_api/response_parser      30      4    0
[TOTAL] 100.0%
```

这看起来与我们使用内置工具时看到的非常相似。覆盖率百分比不同，因为该工具计算代码覆盖率的方式不同，但报告样式相似。如果你也想要 HTML 报告，可以改为运行 `MIX_ENV=test mix coveralls.html`。Excoveralls 还提供其他输出格式，例如 JSON（`MIX_ENV=test mix coveralls.json`）和 XML（`MIX_ENV=test mix coveralls.xml`），允许开发人员以编程方式将测试覆盖率运行的输出集成到不同的系统和平台中。

就集成而言，Excoveralls 提供了几种将覆盖率结果发布到 Coveralls 的方法。你可以从

常见的 CI 平台，例如 CircleCI[59]（mix coveralls.circle）或 Semaphore[60]（mix coveralls.semaphore）。此外，Excoveralls 允许你通过 mix coveralls.post 发布到任何与 Coveralls 兼容的主机。例如，OpenCov[61] 是一个兼容 Coveralls API 的可靠自托管覆盖率平台。

我们简要介绍了测试覆盖率是什么、为什么有用，以及如何将其用于 Elixir 项目。测试覆盖率只是提高对测试套件信心的众多工具之一，但它不一定保证你的测试套件是可靠且写得好的。然而，它可以用来发现代码中测试不足的区域，或者在另一端，发现代码中是“热点路径”且在测试期间被多次执行的区域。根据我们的经验，极高的测试覆盖率（大约 95% 及以上）并不一定意味着代码库健康且功能正常。一旦代码覆盖率足够高，继续努力提高它可能会开始产生递减回报。另一方面，较低的覆盖率（例如大约 40% 或 50%）可能指向测试不足的代码库，可能需要一些改进。总体而言，测试覆盖率的有用性因项目而异，但它是我们测试工具箱中一个有用的工具。

## Footnotes

- [56] https://coveralls.io
- [57] https://coveralls.io
- [58] https://github.com/parroty/excoveralls
- [59] https://circleci.com
- [60] https://semaphoreci.com
- [61] https://github.com/danhper/opencov

### Built-In Test Coverage

ExUnit 提供了一个内置的测试覆盖率工具。你可以通过向 mix test 传递 --cover 标志直接在 Mix 项目中使用它。

让我们使用我们在第 1 章《单元测试》中编写的代码来尝试一下。在终端中打开 unit_tests/soggy_waffle 目录并运行 mix test --cover：

```
> mix test --cover
Cover compiling modules ...
..............

Finished in 1.8 seconds
15 tests, 0 failures

Randomized with seed 384102

Generating cover results ...

| Percentage | Module |
|------------|--------|
| 0.00%      | SoggyWaffle.SmsApi |
| 75.00%     | SoggyWaffle |
| 85.71%     | SoggyWaffle.Weather |
| 100.00%    | SoggyWaffle.WeatherAPI |
| 100.00%    | SoggyWaffle.WeatherAPI.ResponseParser |
|------------|--------|
| 85.00%     | Total |

Generated HTML coverage results in "cover" directory
```

如上输出所示，我们得到了 Mix 项目中包含的模块列表。每个模块旁边都列有一个百分比：这就是我们的代码覆盖率比率。100% 的结果意味着模块中的每一行在测试期间至少执行一次

我们的测试运行。0% 的结果意味着该模块中的代码在测试运行期间未被执行。

内置的代码覆盖率工具还会生成一批 HTML 报告文件（默认在 cover 目录中）。让我们打开 SoggyWaffle.Weather 模块的 HTML 文件，你可以在图中看到它的样子。

```
cover/Elixir.SoggyWaffle.Weather.html

1  defmodule SoggyWaffle.Weather do
2      @type t :: %__MODULE__{}
3
4  4  defstruct [:datetime, :rain?]
5
6
7      @spec imminent_rain?([t()], DateTime.t()) :: boolean()
8  :-(  def imminent_rain?(weather_data, now \\ DateTime.utc_now()) do
9      5  Enum.any?(weather_data, fn
10         %__MODULE__{rain?: true} = weather ->
11      6  in_next_4_hours?(now, weather.datetime)
12
13         _ ->
14         false
15     end)
16     end
17
18     defp in_next_4_hours?(now, weather_datetime) do
19      6  four_hours_from_now =
20         DateTime.add(now, _4_hours_in_seconds = 4 * 60 * 60)
21
22      6  DateTime.compare(weather_datetime, now) in [:gt, :eq] and
23      5  DateTime.compare(weather_datetime, four_hours_from_now) in [:lt, :eq]
24     end
    end
```

阅读这份报告很直接。高亮为绿色的行是在测试运行期间被执行的行。每条绿色行左侧都有一个数字，显示该行被执行的次数。高亮为红色的行是在测试运行期间未被执行的行。所有其他行都是出于各种原因不计入任何覆盖率的行。
例如，函数定义不计入代码覆盖率，多行表达式所在的行也不计入。

类似于由 mix test -- cover 生成的 HTML 报告的代码覆盖率报告，对于发现和解决潜在问题至关重要。只知道模块的覆盖率百分比有助于了解哪些模块可能需要更多关注，但详细的逐行报告使我们能够确切地知道可能缺少哪些测试以提高代码的覆盖率。

覆盖率的几个方面可以通过从 mix.exs 文件的 project/0 函数返回的一组选项进行配置。一个经常有用的选项是 :ignore_modules 选项。在上面的示例中，SoggyWaffle.SmsApi 模块的覆盖率为 0%，因为它是“真正”实现 HTTP API 以访问外部系统的实现，而我们没有任何集成测试或端到端测试在执行它。让我们假装我们不想编写此类测试，并且我们也不想将此特定模块计入总覆盖率。我们可以在 mix.exs 中将其包含在被忽略的模块列表中：

```elixir
defmodule SoggyWaffle.MixProject do
  use Mix.Project

  def project do
    [
      app: :soggy_waffle,
      version: "0.1.0",
      elixir: "~> 1.9",
      start_permanent: Mix.env() == :prod,
      deps: deps(),
      test_coverage: [ignore_modules: [SoggyWaffle.SmsApi]]
    ]
  end
end
```

mix.exs 的其余内容

我们可以在再次运行 `mix test --cover` 时看到这一点：

```
> mix test --cover
Cover compiling modules ...
................

Finished in 1.6 seconds
15 tests, 0 failures

Randomized with seed 969266

Generating cover results ...
```

| 百分比 | 模块 |
|------------|--------|
| 75.00% | SoggyWaffle |
| 85.71% | SoggyWaffle.Weather |
| 100.00% | SoggyWaffle.WeatherAPI |
| 100.00% | SoggyWaffle.WeatherAPI.ResponseParser |
| ---------- | ------------------- |
| 89.47% | Total |

生成的 HTML 覆盖率结果位于 "cover" 目录

`SoggyWaffle.SmsApi` 模块不再出现在报告中，总代码覆盖率上升了几个百分点。
