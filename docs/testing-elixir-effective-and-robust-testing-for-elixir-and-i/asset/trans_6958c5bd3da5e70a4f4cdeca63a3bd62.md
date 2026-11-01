
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
