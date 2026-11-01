Once we have SoggyWaffle.NoOpWeatherAPI, we can use it as a stub of the SoggyWaffle.WeatherAPIMock module in every test where we don’t care about asserting how SoggyWaffle.WeatherAPIMock is called. To do that, it’s enough to call stub_with/2 for those tests:

```
Mox.stub_with(SoggyWaffle.WeatherAPIMock, SoggyWaffle.NoOpWeatherAPI)
```

If we want to default to the no-op module for all our tests and only use the mock module in a few tests that assert how the interface is used, we can use the application environment directly. For example, we can set our application to use the no-op module in the configuration for the test environment:

```
import Mix.Config

config :soggy_waffle, :weather_api_module, SoggyWaffle.NoOpWeatherAPI
```

Then, in the tests where we want to use the mock to verify expectations, we can set the value of :weather_api_module to SoggyWaffle.WeatherAPIMock:

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
  end
```