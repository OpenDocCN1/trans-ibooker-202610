# 第 2 章

## 集成测试和端到端测试

在现实世界中，我们的应用程序仅由纯函数组成的情况很少见。更常见的是，生产应用程序由许多相互交互的组件构成，而且可能最重要的是，还与外部系统交互。我们希望确信所有组件都能协同工作，并且与外部系统的集成也能正常工作。然而，在这种情况下，测试变得更加复杂。例如，假设我们的应用程序使用 Twitter API 发送状态更新作为推文。我们如何在不发布任何推文的情况下以自动化方式测试此行为？我们如何避免超出 Twitter 设定的速率限制？这类问题要求我们在可移动部件数量增加时重新思考如何编写可重复且自动化的测试。

在本章中，我们将学习如何为具有独立组件相互交互以及与外部系统交互的 Elixir 系统设计测试。这种知识，加上我们在前几章中学到的内容，将使我们能够从简单函数和模块的角度以及系统组件之间交互的角度测试系统。

我们将首先快速了解一下涵盖同一应用程序内不同组件交互的集成测试。然后我们将进入更困难的部分，测试我们的应用程序与外部系统的交互。我们将讨论测试外部依赖的不同技术，并了解 Elixir 及其生态系统提供的帮助工具。

## 什么是集成测试？

涵盖不同组件（无论是外部系统还是应用程序的不同部分）集成的测试通常被称为 *integration tests*。在本书中，当我们使用集成测试一词时，我们指的是涵盖调用系统或外部系统两个或更多部分的某段代码的测试。集成测试是测试系统的基础。单元测试让你确信你的函数和模块行为正确；但你需要测试系统整体是否正常运行，而要做到这一点，你需要测试组件之间的粘合部分。让我们从一个集成测试的例子开始。

HTTP 基于的 Web 应用程序中的常见场景是拥有一个根据请求 URI 将请求路由到不同函数的路由器。这些函数通常包含在称为 *controllers* 的模块中。控制器函数负责对请求数据进行必要的处理、制定输出并将响应发送回客户端。路由器和控制器的代码可能如下所示：

```
integration_tests/pseudocode_controller_and_router.ex

defmodule MyApp.Router do
  route("POST", "/perform_action", {MyApp.Controller, :perform_action})
end

defmodule MyApp.Controller do
  def perform_action(connection, params) do
    parsed_params = parse_params(params)
    action_result = perform_action(parsed_params)
    response = build_response(action_result)
    send_response(connection, response)
  end
end
```

我们知道如何编写单元测试。我们可以对单个功能进行单元测试，例如输入的处理（parse_params/1）、控制器执行的隔离操作（perform_action/1）以及响应的构建（build_response/1）。这会让我们确信单个组件工作正常，但我们并不确信这些组件能很好地协同工作：我们需要测试这些组件的集成。为了进入正确的思维框架，想想一架飞机。你可以测试飞机的外壳金属是否抗弯曲、引擎是否正确启动以及飞机的形状是否适合飞行。但如果飞机没有经过测试以确认它是否真的能飞，你会乘坐它吗？

集成测试背后的思想是相同的。我们希望测试我们的组件是否能正确协同工作，即使我们有信心组件本身能正常工作，这要归功于单元测试。在我们的 Web 路由器和控制器示例中，我们希望有一个集成测试，它模拟对 /perform_action 端点使用原始参数的 HTTP 请求，然后断言控制器返回的响应是否正确。

```
integration_tests/pseudocode_controller_and_router_test.exs

defmodule IntegrationTest do
  use ExUnit.Case

  test "POST /perform_action" do
    params = %{"some" => "params"}
    response = simulate_http_call("POST", "/perform_action", params)
    assert response.status == 200
    assert response.body == "OK"
  end
end
```

处理同一系统内组件集成（如本例中的路由器和控制器）的集成测试是最简单的集成测试类型。所有事情都发生在你控制的系统内，因此行为是可预测的。困难开始于我们需要测试系统如何与 *external* 组件接口时。让我们接下来看看这一点。

## 对真实外部依赖进行测试

许多应用程序与数据库、其他服务或第三方 API 接口。这使得集成测试更困难，因为我们现在必须处理其他正在运行的系统，以便设计和运行我们的集成测试套件。在测试中处理外部依赖主要有两种方法：我们可以使用真实的依赖项，或用可以作为依赖项的 *doubles* 替代它。本节中，我们将看看如何通过在测试套件中使用实际依赖项来对外部依赖项进行测试，然后在下一节中查看依赖项双。我们还将提供关于何时每种解决方案合适的指导。

假设我们的应用程序由数据库支持。数据库是应用程序的外部依赖，因为它不在应用程序内部运行。就我们的目的而言，我们可以将其视为一个黑盒，如果使用正确，我们可以假设它能正常工作。让我们想象一下，我们的应用程序使用此数据库存储博客文章，并且它提供 HTTP API 作为创建文章的接口。我们有一个表示文章的 Article 结构体：

```
integration_tests/real_external_dependencies/article.ex

defmodule Article do
  defstruct [:title, :body, :author_email]
end
```

然后我们有一个路由器和控制器来处理 POST /create_article 路由：

```
integration_tests/real_external_dependencies/router_and_controller.ex

defmodule MyApp.Router do
  route(
    "POST",
    "/create_article",
    {MyApp.ArticleController, :create_article}
  )
end

defmodule MyApp.ArticleController do
  def create_article(connection, params) do
    article = %Article{
      title: params["title"],
      body: params["body"],
      author_email: params["author_email"]
    }

    article_id = Database.create(article)

    send_response(connection, _status_code = 200, %{article_id: article_id})
  end
end
```

我们假设我们有一个与数据库接口并公开 Database 模块的库。测试此的最简单方法是，在测试应用程序时启动数据库（在应用程序测试套件运行的机器上），并在执行测试时实际调用它。这样，我们也可以调用数据库以检查我们的代码是否在数据库中正确创建了文章。

```
integration_tests/real_external_dependencies/http_api_test.exs
```

```
defmodule HTTPAPITest do
  use ExUnit.Case

  test "articles are created correctly" do
    params = %{
      "title" => "My article",
      "body" => "The body of the article",
      "author_email" => "me@example.com"
    }

    response = simulate_http_call("POST", "/create_article", params)


```
assert response.status == 200
assert %{"article_id" => article_id} = response.body

assert {:ok, article} = Database.fetch_by_id(Article, article_id)
assert article.title == "My article"
assert article.body == "The body of the article"
assert article.author_email == "me@example.com"
  end
end
```

这个测试可以运行，但我们可能会遇到一个问题：每次执行它时，数据库都会被相同的文章不断填满。良好的做法是在运行测试时始终尝试从干净的状态开始。在这种情况下，这可能意味着在运行测试之前删除所有文章。

这个测试让我们确信代码是可行的，因为它调用的是真实的数据库，并且像在生产环境中一样工作。然而，这些测试的一个缺点是，它们可能比不调用任何外部依赖的测试慢得多。

数据库是我们能够完全控制的系统的外部依赖的一个例子。我们可以随时启动和停止它，并且可以随意修改其中的数据。对于像第三方 API 这样的外部服务来说则并非如此。例如，如果你的应用程序使用 Twitter API，那么我们就不能随时创建推文并调用 API，因为我们需要遵守 API 设定的规则。这种区分——应用程序拥有的外部依赖（如数据库）和不拥有的外部依赖（如第三方 API）——是根本性的。当我们拥有依赖时，通常最好针对正在运行的依赖进行测试，以便使测试套件尽可能接近生产环境中运行的代码。在这些情况下，我们用测试套件的速度换取了代码正确工作的更高信心。

在下一节中，我们将探讨如何处理我们无法控制的外部服务。

## 依赖双

在本节中，我们将了解如何创建一个充当我们无法控制的外部依赖的东西。这将使我们能够在不使用该依赖的情况下测试与外部依赖交互的代码。

在深入之前，有一点免责声明：关于依赖双的术语并不是“标准”的：不同的人以不同的方式使用这些术语。我们将使用 double、mock、stub 等术语，以我们认为最有意义的方式使用并保持一致，但你可能会在本书之外遇到相同的概念但名称不同。我们将在本章后面定义这些术语。话虽如此，让我们开始吧。

充当外部依赖的组件通常被称为该依赖的双。名称来源于这样一个事实：对于依赖的调用者来说，依赖双看起来与依赖完全相同。依赖双的实现复杂度可能各不相同。有时，依赖双看起来像外部依赖但什么都不做。其他时候，依赖双可用于深入了解依赖是如何被调用的，并且可能用于测试交互。让我们深入了解如何在我们的 Soggy Waffle 应用程序中使用依赖双。

Soggy Waffle 需要通过 HTTP 与天气 API 交互，以定期获取天气预报。对我们的应用程序而言，天气 API 是一个外部组件，因此将与之的交互隔离到一个充当应用程序与 API 之间接口的模块中是良好的设计。

```
integration_tests/soggy_waffle/weather_api.ex

defmodule SoggyWaffle.WeatherAPI do
@spec get_forecast(String.t()) ::
  {:ok, map()} | {:error, reason :: term()}
def get_forecast(city) when is_binary(city) do
  app_id = SoggyWaffle.api_key()
  query_params = URI.encode_query(%{"q" => city, "APPID" => app_id})

  url =
    "https://api.openweathermap.org/data/2.5/forecast?" <> query_params

  case HTTPoison.get(url) do
    {:ok, %HTTPoison.Response{status_code: 200} = response} ->
      {:ok, Jason.decode!(response.body)}

    {:ok, %HTTPoison.Response{status_code: status_code}} ->
      {:error, {:status, status_code}}

    {:error, reason} ->
      {:error, reason}
  end
end
end
```

本模块中的 get_forecast/1 函数由我们的主 SoggyWaffle.rain?/2 函数调用：

integration_tests/soggy_waffle/soggy_waffle.ex

```
defmodule SoggyWaffle do
  def rain?(city, datetime) do
    with {:ok, response} <- SoggyWaffle.WeatherAPI.get_forecast(city) do
      weather_data =
        SoggyWaffle.WeatherAPI.ResponseParser.parse_response(response)

      SoggyWaffle.Weather.imminent_rain?(weather_data, datetime)
    end
  end
end
```

我们想要测试 rain?/2 函数。然而，我们无法控制天气 API，因此很难针对它进行测试。如果该 API 宕机，我们的自动化测试（应该是可重复的）将会失败。我们必须找到一种方法来尊重该 API 的速率限制，以便我们的开发人员可以尽可能频繁地运行自动化测试。这些原因表明，我们应该在测试中避免直接联系天气 API。一种常见的方法是用我们在测试期间可以控制的东西替换 SoggyWaffle.WeatherAPI 模块中使用的 HTTP 客户端。然而，这暴露了 SoggyWaffle.WeatherAPI 模块实现的一个细节，而在测试 rain?/2 函数时我们可能并不关心它。让我们上升一个层级，处理 SoggyWaffle.WeatherAPI 暴露的接口。

我们能做的最简单的事情是使用 *依赖注入*，正如我们在第 1 章 *单元测试* 中讨论的那样。当我们之前使用依赖注入时，我们会将一个函数或一个值传递给我们的函数。然而，我们通常将接口封装在模块中，就像我们对 SoggyWaffle.WeatherAPI 所做的那样。在这些情况下，将模块作为我们注入的依赖传递是有意义的。让我们修改 rain?/2 函数，使其接受第三个参数，即“双”模块。

```
integration_tests/soggy_waffle/soggy_waffle_module_di.ex

def rain?(city, datetime, weather_api_module \ SoggyWaffle.WeatherAPI) do
  with {:ok, response} <- weather_api_module.get_forecast(city) do
    weather_data =
      SoggyWaffle.WeatherAPI.ResponseParser.parse_response(response)

    SoggyWaffle.Weather.imminent_rain?(weather_data, datetime)
  end
end
```

在生产环境中，接口模块将是 SoggyWaffle.WeatherAPI。在我们的测试中，它可以是 SoggyWaffle.FakeWeatherAPI。

```
integration_tests/soggy_waffle/fake_weather_api.ex

defmodule SoggyWaffle.FakeWeatherAPI do
  require Logger
@spec get_forecast(String.t()) :: {:ok, map()}
def get_forecast(city) do
  _ = Logger.info("Getting forecast for city: #{city}")

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

在生产代码中，我们在不带第三个参数的情况下调用 rain?/2，默认为真实的天气 API。在测试期间，我们可以用假模块替换它，并通过测试正确的行是否已被记录来断言 SoggyWaffle.FakeWeatherAPI.get_forecast/1 是否被正确调用。

这种依赖注入方法有效，但有一个缺点：每次在测试中调用 rain?/2 函数时，我们都需要记住使用 SoggyWaffle.FakeWeatherAPI 作为第二个参数。当我们测试内部调用 rain?/2 的东西时，这变得尤其痛苦，因为我们需要传播额外的模块参数。幸运的是，Elixir 随带了一些东西，使这种情况下的事情变得更容易：应用程序环境。应用程序环境是一个通常用于配置等内容的键值内存全局存储。我们可以将正在使用的天气 API 模块存储在应用程序环境中，并在执行 rain?/2 时读取它。

integration_tests/soggy_waffle/soggy_waffle_module_from_app_env.ex

```
def rain?(city, datetime) do
  weather_api_module =
```



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

### integration_tests/soggy_waffle/config/test.exs

```
import Mix.Config

config :soggy_waffle, :weather_api_module, SoggyWaffle.FakeWeatherAPI
```

由于我们在测试环境中使用 SoggyWaffle.FakeWeatherAPI，因此我们可以修改测试，以便断言 SoggyWaffle.FakeWeatherAPI 记录的输出：

### integration_tests/soggy_waffle/test/soggy_waffle_test.exs

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

```
stub(SoggyWaffle.WeatherAPIMock, :get_forecast, fn city ->
  Logger.info("Getting forecast for city: #{city}")

  response = %{
    "list" => [
      %{
        "dt" => DateTime.to_unix(DateTime.utc_now()) + 60,
        "weather" => [%{"id" => 231}]
      }
    ]
  }

  {:ok, response}
end)

log =
  capture_log(fn ->
    assert SoggyWaffle.rain?("Los Angeles", DateTime.utc_now())
  end)

assert log =~ "Getting forecast for city: Los Angeles"
end
end
end
```

当像 get_forecast/1 这样的函数被 stub 时，它可以在 mock 模块上被调用任意次数。Stub 通常用于在被 stub 的函数中并不真正需要逻辑，但你希望返回一个值时。例如，假设 get_forecast/1 可以返回 {:error, reason} 来在与天气 API 通信时表示错误。在不严格测试天气 API 功能的测试中，我们可能希望将 get_forecast/1 stub 掉，使其仅返回 :ok，实际上什么都不做。

在我们的例子中，由于我们在被 stub 的函数中执行了一个操作，以便之后可以测试该操作的结果，mock 比 stub 更合适的工具。Mock 的工作方式与 stub 相同，但它允许你断言被 mock 的函数被调用了特定次数。在我们的示例中，我们希望断言 rain?/2 仅调用一次 get_forecast/1。语义上，当你对 mock 模块应如何被调用有一些通用期望时，也会使用 mocks。要创建 mock 函数，Mox 提供了 Mox.expect/4，它接收 mock 模块的名称、函数的名称、被 mock 函数可以被调用的次数，以及一个匿名函数（与 stub/3 类似）。

```
integration_tests/soggy_waffle/test/soggy_waffle_test_with_mox_expect.exs

describe "rain?/2" do
  test "success: gets forecasts, returns true for imminent rain" do
    expect(SoggyWaffle.WeatherAPIMock, :get_forecast, 1, fn city ->
      assert city == "Los Angeles"

      response = %{
        "list" => [
          %{
            "dt" => DateTime.to_unix(DateTime.utc_now()) + 60,
            "weather" => [%{"id" => 231}]
          }
        ]
      }

      {:ok, response}
    end)

    assert SoggyWaffle.rain?("Los Angeles", DateTime.utc_now())

    verify!(SoggyWaffle.WeatherAPIMock)
  end
end
```

我们将对 stub/3 函数的调用改为对 expect/4 的调用。我们期望 get_forecast/1 仅被调用一次，因此我们使用 1 作为期望的调用次数（这是默认值，但我们喜欢明确指定）。传递给 expect/4 的匿名函数中，我们对期望 mock 的调用方式进行了断言。通过结合期望的调用次数和匿名函数中的断言，我们为 rain?/2 应如何使用天气 API 接口设定了明确的期望。

使用 stub/3 和使用 expect/4 之间还有一个区别。当我们使用 expect/4 时，我们需要在测试结束时调用 Mox.verify!/1。verify!/1 接收一个 mock 模块的名称（在本例中为 SoggyWaffle.WeatherAPIMock）并验证我们在 mock 函数上设定的期望。如果我们调用 mock 函数的次数少于或多于 expect/4 中指定的次数，除非我们调用 verify!/1，否则不会报错。

在这种情况下，mock 是测试 rain?/2 函数是否正确使用天气 API 接口的最简洁解决方案。使用 fake 或 stub 时，我们不得不求助于 Logger 调用（这是一个全局副作用）来测试天气 API 接口是否被正确次数调用以及使用正确的参数。然而，使用 mock，我们可以将断言直接放在 mock 函数中就完事了。

让我们快速聊聊验证期望。在实际代码中，手动调用 verify!/1 并不是很常见，因为 Mox 提供了一些更少重复的替代方案。第一个方案并没有更简洁，但可以稍微简化一些：我们不用带 mock 模块名称调用 verify!/1，而是可以调用不带参数的 Mox.verify!/0。这样做时，Mox 将验证在调用它的测试中使用的所有 mock 模块上的所有期望。另一个有用的函数是 Mox.verify_on_exit!/1。调用时，该函数为当前测试设置一个 on_exit hook，在测试进程退出时验证测试中的所有期望（与 verify!/0 类似）。verify_on_exit!/1 接收一个可选参数，即测试上下文。这是为了方便，因为使用 verify_on_exit!/1 的一个好方法是将其用作 setup 回调。

```
integration_tests/soggy_waffle/test/soggy_waffle_test_verify_on_exit_setup.exs

import Mox

setup :verify_on_exit!
```

在全局测试上下文中使用时，Mox 将在每个测试结束时验证期望。如果我们想要更多控制权，我们始终可以使用 describe 块（如在 Describing Your Tests 中所述），并仅在需要的地方使用 setup 回调。

## 允许从不同进程使用 Mocks

我们尚未讨论 Mox 如何决定谁被允许调用 mock 或 stub 函数。当一个 double 的函数被 stub 或 mock 时，该 stub 或 mock 默认仅在该函数由定义 stub 或 mock 的进程调用时才有效。在我们的天气 API 示例中，这不会造成任何问题，因为天气 API 接口是由调用 rain?/2 的进程直接使用的，而该进程就是测试进程。然而，让我们稍微修改 rain?/2 函数的代码，以便在不同的进程中调用天气 API 接口。我们将使用 Task.async/1 来生成新进程，并使用 Task.await/1 来阻塞直到天气 API 的调用完成。我们确实是在强迫代码使用另一个进程，因为在这种特定情况下异步调用 API 并不合理，但这有助于展示此 Mox 功能，请稍作忍耐：

```
integration_tests/soggy_waffle/soggy_waffle_async_get_forecast.ex

def rain?(city, datetime) do
  weather_api_module =
    Application.get_env(
      :soggy_waffle,
      :weather_api_module,
      SoggyWaffle.WeatherAPI
    )

  weather_api_task =
    Task.async(fn ->
      weather_api_module.get_forecast(city)
    end)

with {:ok, response} <- Task.await(weather_api_task) do
  weather_data =
    SoggyWaffle.WeatherAPI.ResponseParser.parse_response(response)

  SoggyWaffle.Weather.imminent_rain?(weather_data, datetime)
end
end
```

如果我们尝试像之前一样运行相同的测试，Mox 将失败，并提示 SoggyWaffle.WeatherAPIMock 不能从测试进程外部调用。对于这种情况，Mox 提供了两个解决问题的函数。第一个是 Mox.allow/3：它接收 mock 模块名称、定义 stubs 或 mocks 的进程的 PID，以及随后被允许使用由定义进程定义的相同 stubs 和 mocks 的进程的 PID。实际上，这个函数并不常用，因为它要求你知道想要使用 mock 模块的进程的 PID。在我们的例子中，我们在 rain?/2 函数外部不知道天气 API 任务的 PID，因此无法使用 allow/3。幸运的是，Mox 还提供了 Mox.set_mox_global/1 函数。当 Mox 被设置为全局模式时，mock 模块可以从任何进程调用。这正是我们在示例中想要的。我们可以直接调用 set_mox_global/1，但该函数可以像 verify_on_exit!/1 一样使用，即作为 setup 回调。

```
integration_tests/soggy_waffle/test/soggy_waffle_test_set_mox_global_setup.exs

import Mox

setup :set_mox_global
setup :verify_on_exit!
```

为了对比 set_mox_global/1，Mox 还提供了 Mox.set_mox_private/1，它将 Mox 设置为私有模式（默认值）。相比私有模式或 allow/3 调用，使用 Mox 的全局模式有一个缺点：测试用例不能是异步的。这可能会使测试套件稍微变慢，但在像 rain?/2 这样的情况下，我们别无选择。



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



## 测试与服务的实际交互

在处理像天气 API 这样的第三方 API 时，我们会将与 API 集成的代码推送到应用程序外部的轻量级包装器中。在测试我们的应用程序时，我们能够用一个双重对象替换这小部分来运行应用程序的其余部分。不过，在某些时候，我们也需要确保与 API 通信的代码能够正常工作。在本节中，我们将学习实现这一目标的不同方法。

在我们的天气 API 应用程序中，与天气 API 通信的代码如下：

```
integration_tests/soggy_waffle/weather_api.ex

defmodule SoggyWaffle.WeatherAPI do
  @spec get_forecast(String.t()) ::
    {:ok, map()} | {:error, reason :: term()}
  def get_forecast(city) when is_binary(city) do
    app_id = SoggyWaffle.api_key()
    query_params = URI.encode_query(%{"q" => city, "APPID" => app_id})

    url =
      "https://api.openweathermap.org/data/2.5/forecast?" <> query_params

    case HTTPoison.get(url) do
      {:ok, %HTTPoison.Response{status_code: 200} = response} ->
        {:ok, Jason.decode!(response.body)}

      {:ok, %HTTPoison.Response{status_code: status_code}} ->
        {:error, {:status, status_code}}

      {:error, reason} ->
        {:error, reason}
    end
  end
end
```

我们使用 HTTPoison 作为 HTTP 客户端。get/1 函数允许我们向传递给它的 URL 发送 GET HTTP 请求并返回一个 {:ok, response} 元组。然而，我们仍然希望确保 URL 对我们的请求是正确的，请求被正确执行，并且响应实际上是一个 200 状态码以及我们能够解析的正文。除此之外别无他法，只能向天气 API 发送真实请求，但有几种方法可以可靠且可重复地做到这一点。

### 发送真实请求

最简单的方法是在测试中向天气 API 发送真实请求并断言 API 返回的响应。这种方法的优点是我们直接测试真实 API，因此如果 API 发生变化或与其交互的代码发生变化，测试可能会失败。然而，存在一些重要的缺点。首先，我们的测试现在依赖于我们无法控制的第三方系统的可用性。如果天气 API 宕机或我们无法访问互联网，即使代码正确，测试也会失败，这意味着我们的测试是脆弱且不可重复的。另一个缺点是，API 可能更改测试所依赖的某些返回数据，而不会破坏契约。在这种情况下，测试可能会开始失败，而不会发出真实问题的信号。这种方法的另一个主要缺点是，发出真实的 HTTP 请求在许多用例中会引起问题。例如，我们使用的天气 API 受到速率限制（大多数 API 在某种程度上都是如此），这意味着我们的测试可能会影响 API 的速率限制，而不会为用户提供服务。

在某些情况下，真实请求方法的缺点是可以得到缓解。例如，一些第三方 API 提供“staging”或“sandbox” API，其接口与真实 API 相同但行为不同。例如，对于我们使用的类似天气 API，sandbox API 可以始终返回相同的天气数据，而无需实际与任何预测服务通信。这将显著减少对天气 API 本身的负载，从而允许其开发人员在此环境中可能取消速率限制。然而，许多第三方 API 都没有提供类似的东西，因此我们必须想办法避免在测试中发出真实请求，但仍然测试发出这些请求的代码。让我们接下来看看两种最常见的方法。

### 构建临时 HTTP 服务器

我们要检查的第一个方法是构建一个临时 HTTP 服务器，该服务器与我们的测试套件一起运行，我们可以向其发送请求。我们将控制服务器本身，因此我们能够控制其行为，并在测试期间向其发送任意数量的请求。

让我们回到天气 API 的用例。天气 API 暴露了一个 GET /data/2.5/forecast 端点，我们的应用程序会调用它。该端点在查询字符串中接受两个参数：q，即查询，以及 APPID，用于标识我们应用程序的凭据。该端点返回一个 200 OK HTTP 响应，其中包含有关天气预报信息的 JSON 正文。让我们构建一个模仿此 API 的 HTTP 服务器。

Elixir 生态系统拥有构建 HTTP 服务器的成熟库。目前，我们将使用 Plug.[15] Plug 是一个提供不同 Erlang 和 Elixir Web 服务器通用接口的库。常用的 Web 服务器是 Cowboy.[16] 我们需要做的第一件事是向依赖项中添加 Plug 和 Cowboy，这可以通过添加 :plug_cowboy 依赖项来完成。我们只会在 :test 环境中添加此依赖项，以便它不会随我们的应用程序一起发布（假设我们的应用程序本身不使用 Plug 和 Cowboy）：

```
integration_tests/soggy_waffle_actual_integrations/mix_with_plug_cowboy.exs

defp deps do
  [
    # other dependencies
    {:plug_cowboy, ">= 0.0.0", only: :test}
  ]
end
```

现在让我们定义一个暴露端点的 Plug。我们将使用 Plug.Router，它提供了一种直观的 DSL 来编写简单的 HTTP 端点：

```
integration_tests/soggy_waffle_actual_integrations/test/support/weather_api_test_router.exs

defmodule SoggyWaffle.WeatherAPITestRouter do
  use Plug.Router

  # We need to manually import the assertions since we're not
  # inside an ExUnit test case.
  import ExUnit.Assertions

  plug :match
  plug :dispatch
  plug :fetch_query_params

  get "/data/2.5/forecast" do
    params = conn.query_params

    assert is_binary(params["q"])
    assert is_binary(params["APPID"])

    forecast_data = %{
      "list" => [
        %{
          "dt" => DateTime.to_unix(DateTime.utc_now()),
          "weather" => [%{"id" => _thunderstorm = 231}]
        }
      ]
    }

    conn
    |> put_resp_content_type("application/json")
    |> send_resp(200, Jason.encode!(forecast_data))
  end
end
```

我们有一个服务器，它暴露了一个与天气 API 完全相同的端点，但会对传入请求执行一些断言。我们需要启动服务器。在 SoggyWaffle.WeatherAPI 测试用例的 setup 回调中执行此操作：

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/api_test_plug_cowboy.exs

setup do
  options = [
    scheme: :http,
    plug: SoggyWaffle.WeatherAPITestRouter,
    options: [port: 4040]
  ]

  start_supervised!({Plug.Cowboy, options})
  :ok
end
```

我们在端口 4040 上启动了一个 HTTP 服务器，我们可以通过访问 http://localhost:4040/data/2.5/forecast 在整个测试中使用它。然而，真实的天气 API URL（https://api.openweathermap.org/data/2.5/forecast）已硬编码在 SoggyWaffle.WeatherAPI 模块中。我们需要使其可配置。我们可以使用与处理双重对象时将模块作为可选参数传递时相同的方法。让我们更改 SoggyWaffle.WeatherAPI：

```
integration_tests/soggy_waffle_actual_integrations/lib/soggy_waffle/weather_api.ex

defmodule SoggyWaffle.WeatherAPI do
  @default_base_url "https://api.openweathermap.org"

  @spec get_forecast(String.t(), String.t()) ::
    {:ok, map()} | {:error, reason :: term()}
  def get_forecast(city, base_url \\ @default_base_url)
    when is_binary(city) do
    app_id = SoggyWaffle.api_key()
    query_params = URI.encode_query(%{"q" => city, "APPID" => app_id})
    url = base_url <> "/data/2.5/forecast?" <> query_params

    case HTTPoison.get(url) do
      {:ok, %HTTPoison.Response{status_code: 200} = response} ->
        {:ok, Jason.decode!(response.body)}

      {:ok, %HTTPoison.Response{status_code: status_code}} ->
        {:error, {:status, status_code}}

      {:error, reason} ->
        {:error, reason}
    end
  end
end
```

现在我们可以添加一个针对 SoggyWaffle.WeatherAPI 的测试，该测试会命中临时测试服务器：

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/api_test_plug_cowboy.exs

test "get_forecast/1 hits GET /data/2.5/forecast" do
  query = "losangeles"
  app_id = "MY_APP_ID"
  test_server_url = "http://localhost:4040"

  assert {:ok, body} =
    SoggyWaffle.WeatherAPI.get_forecast(
      "Los Angeles",
      test_server_url
    )

  assert %{"list" => [weather | _]} = body
  assert %{"dt" => _, "weather" => _} = weather
  # potentially more assertions on the weather
end
```

此测试将在每次运行时命中测试服务器并断言 SoggyWaffle.WeatherAPI.get_forecast/1 函数是否命中了正确的端点。从头开始编写自己的服务器效果很好，但仍有改进的空间

改进。例如，在我们的测试服务器中，我们只断言 "q" 和 "APPID" 参数是字符串，但并未检查它们是否与测试中指定的字符串相同。要做到这一点，我们必须在测试服务器代码中硬编码这些字符串，这反过来意味着我们必须为测试不同场景而构建新的测试服务器。有一种名为 Bypass 的工具可以帮助解决这种情况。

Bypass 是一个库，允许你即时定义基于 Plug 的服务器，其 API 与我们在前几节中看到的 Mox 提供的 API 相似。让我们看看如何使用它来改进我们的测试。[17] 首先，将该库添加到你的依赖项中，以替代 :plug_cowboy：

```
integration_tests/soggy_waffle_actual_integrations/mix_with_bypass.exs

defp deps do
  [
    # other dependencies
    {:bypass, ">= 0.0.0", only: :test}
  ]
end
```

Bypass 提供了 Bypass.expect_once/4 来为请求设置期望。要使用此函数，我们需要在测试中打开一个 Bypass 连接。

在 setup 回调中执行此操作：

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/weather_api_test_bypass.exs

setup do
  bypass = Bypass.open()
  {:ok, bypass: bypass}
end
```

我们从测试返回一个 bypass 数据结构，其中将包含诸如服务器启动端口之类的信息。我们会通过测试上下文在测试中传递此数据结构，然后将其传递给我们在 Bypass 模块上调用的函数，以便它们知道如何与测试交互。

现在我们可以使用 Bypass 期望重写 get_forecast/1 的测试：

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/weather_api_test_bypass.exs
```

```
test "get_forecast/1 hits GET /data/2.5/forecast", %{bypass: bypass} do
  query = "losangeles"
  app_id = "MY_APP_ID"
  test_server_url = "http://localhost:4040"

  forecast_data = %{
    "list" => [
      %{
        "dt" => DateTime.to_unix(DateTime.utc_now()) + _seconds = 60,
        "weather" => [%{"id" => _thunderstorm = 231}]
      }
    ]
  }

  Bypass.expect_once(bypass, "GET", "/data/2.5/forecast", fn conn ->
    conn = Plug.Conn.fetch_query_params(conn)

    assert conn.query_params["q"] == query
    assert conn.query_params["APPID"] == app_id

    conn
    |> Plug.Conn.put_resp_content_type("application/json")
    |> Plug.Conn.resp(200, Jason.encode!(forecast_data))
  end)

  assert {:ok, body} =
    SoggyWaffle.WeatherAPI.get_forecast(
      "Los Angeles",
      test_server_url
    )

  assert body == forecast_data
```

```
end
```

Bypass.expect_once/4 期望指定的请求恰好发出一次。传递给它的函数接收一个 conn 数据结构（一个 Plug.Conn 结构体），我们可以用它来进行断言并发送响应。如你所见，此 API 与 Mox 提供的 API 相似，允许我们对测试服务器进行细粒度控制，并在每个测试中设置不同的期望。

这种“真实请求”方法的好处是允许我们在测试期间发送任意多的真实 HTTP 请求，以便我们可以[content]
改进。例如，在我们的测试服务器中，我们只断言 "q" 和 "APPID" 参数是字符串，但并未检查它们是否与测试中指定的字符串相同。要做到这一点，我们必须在测试服务器代码中硬编码这些字符串，这反过来意味着我们必须为测试不同场景而构建新的测试服务器。有一种名为 Bypass 的工具可以帮助解决这种情况。

Bypass 是一个库，允许你即时定义基于 Plug 的服务器，其 API 与我们在前几节中看到的 Mox 提供的 API 相似。让我们看看如何使用它来改进我们的测试。[17] 首先，将该库添加到你的依赖项中，以替代 :plug_cowboy：

```
integration_tests/soggy_waffle_actual_integrations/mix_with_bypass.exs

defp deps do
  [
    # other dependencies
    {:bypass, ">= 0.0.0", only: :test}
  ]
end
```

Bypass 提供了 Bypass.expect_once/4 来为请求设置期望。要使用此函数，我们需要在测试中打开一个 Bypass 连接。

在 setup 回调中执行此操作：

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/weather_api_test_bypass.exs

setup do
  bypass = Bypass.open()
  {:ok, bypass: bypass}
end
```

我们从测试返回一个 bypass 数据结构，其中将包含诸如服务器启动端口之类的信息。我们会通过测试上下文在测试中传递此数据结构，然后将其传递给我们在 Bypass 模块上调用的函数，以便它们知道如何与测试交互。

现在我们可以使用 Bypass 期望重写 get_forecast/1 的测试：

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/weather_api_test_bypass.exs
```

```
test "get_forecast/1 hits GET /data/2.5/forecast", %{bypass: bypass} do
  query = "losangeles"
  app_id = "MY_APP_ID"
  test_server_url = "http://localhost:4040"

  forecast_data = %{
    "list" => [
      %{
        "dt" => DateTime.to_unix(DateTime.utc_now()) + _seconds = 60,
        "weather" => [%{"id" => _thunderstorm = 231}]
      }
    ]
  }

  Bypass.expect_once(bypass, "GET", "/data/2.5/forecast", fn conn ->
    conn = Plug.Conn.fetch_query_params(conn)

    assert conn.query_params["q"] == query
    assert conn.query_params["APPID"] == app_id

    conn
    |> Plug.Conn.put_resp_content_type("application/json")
    |> Plug.Conn.resp(200, Jason.encode!(forecast_data))
  end)

  assert {:ok, body} =
    SoggyWaffle.WeatherAPI.get_forecast(
      "Los Angeles",
      test_server_url
    )

  assert body == forecast_data
```

```
end
```

Bypass.expect_once/4 期望指定的请求恰好发出一次。传递给它的函数接收一个 conn 数据结构（一个 Plug.Conn 结构体），我们可以用它来进行断言并发送响应。如你所见，此 API 与 Mox 提供的 API 相似，允许我们对测试服务器进行细粒度控制，并在每个测试中设置不同的期望。

这种“真实请求”方法的好处是允许我们在测试期间发送任意多的真实 HTTP 请求，以便我们可以锻炼与真实 HTTP API 以及我们正在使用的 HTTP 客户端交互的代码。然而，这种方法也有缺点。当构建测试服务器并设置请求期望时，我们实际上是在复制第三方 API 的行为，通过这样做，我们将自己绑定到该 API 的特定行为。如果天气 API 发生变化，而我们仅依赖基于测试服务器的测试，在运行测试套件时我们不会注意到变化。这很重要，因为对于这个问题没有明确且直接的解决方案。唯一的办法是定期检查天气 API 是否仍以与测试服务器相同的方式运行。我们可以手动执行此操作，也可以不时地将代码与真实的天气 API 相对照运行。

在下一节中，我们将看到一种替代方法来解决相同的问题，该方法在某些方面做出妥协，以便更轻松地保持测试的最新状态。

## 使用磁带录制请求

到目前为止，我们已经探索了两种用于测试与第三方 API 交互的替代方案：向真实 API 发出请求或在测试期间构建测试服务器来模拟第三方 API。在本节中，我们将探索最后一个方法，该方法通过一个名为 ExVCR 的库来记录和重放请求。[18]

ExVCR 背后的想法是第一次向真实的第三方 API 发出请求并将响应记录到一个名为 *磁带* 的文件中。然后，当我们需要再次向第三方 API 发出相同请求时，ExVCR 将 *重放* 该请求并从磁带返回响应，而不会发出任何真实的 HTTP 调用。到现在你可能明白为什么它被称为 ExVCR：磁带、录制、重放……很有道理。

ExVCR 的工作方式是通过创建广泛使用的 Erlang 和 Elixir HTTP 客户端的隐式模拟，例如内置的 httpc 或 hackney。[19][20] 这些模拟拦截请求，如果没有对应的磁带则记录它们，或从相应的磁带重放请求。这种方法在你不使用 ExVCR 支持的 HTTP 客户端之一时存在局限性，因为 ExVCR 将无法工作。然而，许多应用程序确实使用 ExVCR 支持的客户端，因此仍然值得探索。

让我们看看如何更改 SoggyWaffle.WeatherAPI 测试以使用 ExVCR。首先将 :ex_vcr 添加为依赖项：

```
integration_tests/soggy_waffle_actual_integrations/mix_with_ex_vcr.exs

defp deps do
  [
    # other dependencies
    {:ex_vcr, ">= 0.0.0", only: :test}
  ]
end
```

get_forecast/1 函数使用 HTTP poison 作为其 HTTP 客户端，而 HTTP poison 在内部使用 :hackney，因此 ExVCR 将有效。现在我们需要调用 use ExVCR.Mock 使 ExVCR DSL 在我们的测试中可用，并且我们将使用 ExVCR.Adapter.Hackney 适配器。

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/weather_api_test_ex_vcr.exs
```

use ExVCR.Mock, adapter: ExVCR.Adapter.Hackney

ExVCR 提供了 use_cassette/2 宏，它接受一个磁带名称和一段代码块。代码块中执行的请求将被记录到指定的磁带并从中重放。让我们重写 get_forecast/1 测试以使用 use_cassette/2。

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/weather_api_test_ex_vcr.exs
```

```
test "get_forecast/1 hits GET /data/2.5/forecast" do
  query = "losangeles"
  app_id = "MY_APP_ID"

  use_cassette "weather_api_successful_request" do
    assert {:ok, body} =
      SoggyWaffle.WeatherAPI.get_forecast("Los Angeles")
  end

  assert %{"list" => [weather | _]} = body
  assert %{"dt" => _, "weather" => _} = weather
  # potentially more assertions on the weather
end
```

第一次运行此测试时，请求将发送到真实的天气 API。天气 API 返回响应后，该响应将被记录到名为 weather_api_successful_request 的磁带中。我们使用描述性和唯一的名称，以便它不会与其他磁带冲突。当再次运行相同的测试时，不会发出任何 HTTP 请求，并且记录在磁带中的响应将被返回给 HTTP 客户端。

这种方法与测试服务器不同，因为它更少关注断言请求是否正确发出。磁带的主要目标是完全像真实的第三方服务一样行为，而无需编写代码来模拟该第三方服务。工作流程实际上比测试服务器更简单：我们只需用 use_cassette/2 包裹我们的代码即可。然而，磁带与测试服务器存在类似的问题，即它们可能与实际 API 不同步。对于磁带，解决方案稍微简单一些，因为我们只需要删除过时的磁带并重新运行测试即可重新创建最新的磁带。

为了进一步推动保持磁带最新的想法，我们始终可以在连续集成（CI）服务器上运行测试时强制发出真实请求。这样，我们在本地机器上开发时可以避免发出真实的 HTTP 请求，但 CI 服务器（通常运行频率较低）将确保磁带没有过时。这种方法在很大程度上取决于向真实 API 发出请求意味着什么。在天气 API 示例中，向 /data/2.5/forecast 发出真实请求是可行的：如果只在 CI 中执行，则不太可能对我们的速率限制产生负面影响。在其他情况下，发出请求可能会花费金钱或破坏事物，因此在每次 CI 运行中发出真实请求可能并不理想。此外，我们通常希望 CI 可重现且在运行之间保持一致，而依赖外部 API 的可用性可能不可行。

我们最喜欢的磁带用例是一个允许你通过其 API 设置和拆除资源的外部服务。例如，天气 API 可能会公开用于注册和删除命名查询的端点。现在，如果我们想测试是否可以通过命名查询查询预报，我们可以在同一个测试中创建命名查询、测试适当的函数，然后删除命名查询。在这种用例中，磁带仅仅成为 HTTP 请求的“缓存”。即使不使用磁带（例如在 CI 中），测试也会创建和销毁运行测试所需的所有资源，从而保持外部服务的状态不变。

在使用 ExVCR 时需要牢记的一个重要做法是永远不要重用磁带。如果你有两个测试发出相同的请求，你可能会想使用相同的磁带，但很可能在某个时候其中一个测试发出的请求会略有变化，然后事情就不会再工作了。如果你

在每个测试中使用不同的磁带，你就能保证在这方面不会搞乱。

让我们回顾一下本节讨论的内容，并看看哪种方法最适合不同的使用场景。

## What Approach Is Best for Testing External Services?
## 测试外部服务的最佳方法是什么？

We saw three possible approaches to testing third-party HTTP services: making real requests, running a test server that mocks the service, or recording real requests and replaying them. As is often the case in our industry, there’s no definitive best approach among these. Each one fits some use cases better.
我们看到了三种可能的第三方 HTTP 服务测试方法：发出真实请求、运行模拟服务的测试服务器，或记录真实请求并重放它们。正如我们行业中经常出现的情况一样，这几种方法中没有绝对的最佳方法。每种方法都更适合某些使用场景。

If you can get away with making real requests to a service, then it’s often a good idea to do that. Many HTTP services provide sandbox environments specifically for testing or are idempotent, meaning that you can make requests without affecting the state of the service. Even in these cases, though, you might want to avoid the real requests approach if you don’t want to rely on an Internet connection in your tests. Nowadays it’s rare to not have an Internet connection available, but there are situations (like being on a flight) that take your Internet connection away. Relying on an Internet connection can also cause all sorts of weird problems if the connection is unstable. Personally, we’re fans of a workaround that gives you a nice way to deal with this: we tag our tests that use the Internet as @tag :requires_internet. This way, if you don’t have a connection available, you can run `mix test --exclude requires_internet` and still run all the tests that don’t require the connection. This is a good workaround if you’re rarely without a connection. One thing to consider is that this approach doesn’t necessarily let you test different kinds of responses from an external service (such as temporary errors), so you might have to mix this in with other approaches.
如果可以发出真实请求到服务，那么通常这样做是个好主意。许多 HTTP 服务提供专门用于测试的沙箱环境，或是幂等的，这意味着你可以发出请求而不影响服务的状态。即便如此，如果你不想在测试中依赖互联网连接，你可能仍想避免真实请求的方法。如今没有互联网连接的情况很少见，但确实存在（例如在飞行中）会失去互联网连接的情况。依赖互联网连接如果连接不稳定也会导致各种奇怪的问题。个人而言，我们喜欢一种变通方法，可以很好地处理这个问题：我们将使用互联网的测试标记为 @tag :requires_internet。这样，如果你没有可用的连接，可以运行 `mix test --exclude requires_internet` 仍然运行所有不需要连接的测试。如果你很少没有连接，这是一个不错的变通方法。需要考虑的一点是，这种方法不一定能让你测试外部服务的不同响应类型（例如临时错误），因此你可能需要将其与其他方法混合使用。

If you want tight control over how the third-party API should behave, then building a test server is the way to go. This lets you test different responses from the API and gives you flexibility. However, the trade-off is that you have to build the test server and then keep it up to date with the real API.
如果你想对第三方 API 的行为有严格控制，那么构建测试服务器是正确的方法。这让你可以测试 API 的不同响应并提供灵活性。然而，代价是你必须构建测试服务器，然后将其与真实 API 保持更新。

If you want to mimic the API perfectly and use one of the supported HTTP clients, then ExVCR is a good alternative. The trade-off is that you have to keep cassettes up to date, but that’s something that you can only avoid by going with the real requests approach anyway.
如果你想完美模拟 API 并使用其中一种受支持的 HTTP 客户端，那么 ExVCR 是一个不错的替代方案。代价是你必须保持磁带更新，但这只能通过采用真实请求方法来避免。

## Testing Non-HTTP Services
## 测试非 HTTP 服务

In the previous sections, we mostly talked about testing HTTP-based third-party services. We explored how to build doubles for those services and then how to test the interaction with the actual services. The concepts we discussed mostly apply to non-HTTP services as well. If you’re talking with a service that exposes a binary protocol, for example, you can build an interface (with a behaviour) that specifies how to interact with the service. Then you can use dependency doubles to test components that talk to the service through the interface. However, testing the actual interaction with a service that doesn’t expose an HTTP interface is a bit more complex because fewer tools are available. If you want to build a test server, you’ll often have to implement that yourself on top of lower-level protocols like TCP. If you want cassettes, you’ll have to implement all of the infrastructure needed for them yourself because there isn’t an ExVCR counterpart for protocols other than HTTP.
在前面的章节中，我们主要讨论了基于 HTTP 的第三方服务的测试。我们探讨了如何为这些服务构建双件，然后如何测试与实际服务的交互。我们讨论的概念大多也适用于非 HTTP 服务。例如，如果你与一个暴露二进制协议的服务通信，你可以构建一个接口（带有一个行为）来指定如何与服务交互。然后你可以使用依赖双件来测试通过接口与服务通信的组件。然而，测试与不暴露 HTTP 接口的服务实际交互会复杂一些，因为可用工具更少。如果你想构建测试服务器，你通常必须在 TCP 等低级协议之上自行实现。如果你想要磁带，你必须自行实现所有所需的基础设施，因为除了 HTTP 之外没有 ExVCR 的对应物。

Luckily, most third-party services provide HTTP APIs. Usually, the external systems that do not provide HTTP APIs are the ones we have control over, such as databases or message queues.
幸运的是，大多数第三方服务提供 HTTP API。通常，不提供 HTTP API 的外部系统是我们控制的系统，例如数据库或消息队列。

Now that we have a good mental image of how to test external services, let’s quickly discuss end-to-end tests.
现在我们对如何测试外部服务有了良好的概念，让我们快速讨论端到端测试。

## End-to-End Tests
## 端到端测试

*End-to-end tests* are tests that sit one level higher than integration tests. Instead of testing the integration between components of the system or between the system and external dependencies, end-to-end tests exercise the whole system *from end to end* as it might be used by the final user.
*端到端测试* 是比集成测试高一级的测试。它们不是测试系统组件之间或系统与外部依赖之间的集成，而是像最终用户使用那样对整个系统进行 *从头到尾* 的演练。

Let’s look at the weather API example from a wider perspective and think about the whole system. End-to-end tests should test the system from the perspective of a user. In this case, an end-to-end test would spin up the system and perform assertions on the observable changes. For example, the end-to-end test could start the system and then wait for an alert about rain.
让我们从更广的角度来看天气 API 示例并思考整个系统。端到端测试应从用户角度测试系统。在这种情况下，端到端测试会启动系统并对可观察的变化进行断言。例如，端到端测试可以启动系统然后等待关于降雨的警报。

The principle behind end-to-end tests is to test the real system, so using dependency doubles rarely makes sense. However, the architecture induced by dependency doubles is still useful, since we can swap the doubles for the real external dependencies when running end-to-end tests.
端到端测试背后的原则是测试真实系统，因此使用依赖双件很少有意义。然而，依赖双件带来的架构仍然有用，因为在运行端到端测试时我们可以将双件替换为真实的外部依赖。

Unfortunately, it’s hard to find patterns for end-to-end tests when it comes to interfacing with external dependencies, since finding patterns often depends on the nature of the external dependency as well as how the system’s allowed to use it.
不幸的是，就接口与外部依赖而言，很难找到端到端测试的模式，因为找到模式通常取决于外部依赖的性质以及系统被允许如何使用它。

It’s also hard to find patterns when it comes to an end-to-end test pretending to be a user of the system under test. As with the interaction with external systems, this heavily depends on the nature of the system we’re testing. For example, if we’re testing a system that exposes an HTTP API, then we might want to use an HTTP client in the end-to-end tests and perform actual requests to the systems like a client of the API would. If our system is a website that you can visit from the browser, we’d use tools that can script human-like browser interactions. We’ll have a deeper look at some of these approaches in Chapter 6, [Testing Phoenix](Testing Phoenix).
在端到端测试假装成为被测系统的用户时，也很难找到模式。就像与外部系统的交互一样，这在很大程度上取决于我们测试的系统的性质。例如，如果我们测试的系统暴露了 HTTP API，那么我们可能希望在端到端测试中使用 HTTP 客户端并像 API 客户端那样对系统执行实际请求。如果我们的系统是一个可以通过浏览器访问的网站，我们会使用可以脚本化类人类浏览器交互的工具。我们将在第 6 章 [测试 Phoenix](Testing Phoenix) 中更深入地研究其中一些方法。

## Wrapping Up
## 总结

We had a look at the practice of integration testing and end-to-end testing. We discussed in-depth ways to write integration tests for parts of the system that interface with external systems, and we looked at some Elixir-specific tooling for doing so. In the next chapter, we’ll look at testing one of the most peculiar tools in the Elixir and Erlang landscapes, the OTP set of abstractions. There we’ll see some of the things mentioned in this chapter being used in more practical scenarios.
我们研究了集成测试和端到端测试的实践。我们深入讨论了为与外部系统接口的系统部分编写集成测试的方法，并查看了一些 Elixir 特定的工具。在下一章中，我们将研究 Elixir 和 Erlang 生态系统中一个最特别的工具，即 OTP 抽象集合。在那里我们将看到本章提到的一些内容在更实际的场景中被使用。

## Footnotes
## 脚注

- [14] https://github.com/plataformatec/mox
- [15] https://github.com/elixir-plug/plug
- [16] https://github.com/ninenines/cowboy
- [17] https://github.com/PSPDFKit-labs/bypass
- [18] https://github.com/parroty/exvcr
- [19] http://erlang.org/doc/man/httpc.html
- [20] https://github.com/benoitc/hackney

# Chapter 3