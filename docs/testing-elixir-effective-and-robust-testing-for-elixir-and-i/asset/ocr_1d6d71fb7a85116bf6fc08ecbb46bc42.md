```
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

In production code, we call rain?/2 without a third argument, defaulting to the real weather API. During testing, we can swap that out with the fake module and assert that SoggyWaffle.FakeWeatherAPI.get_forecast/1 has been called correctly by testing that the correct line has been logged.

This approach to dependency injection works but has a drawback: every time we call the rain?/2 function in tests, we need to remember to use SoggyWaffle.FakeWeatherAPI as the second argument. This becomes especially painful when we’re testing something that internally calls out to rain?/2, because we need to propagate the additional module argument. Luckily for us, Elixir ships with something that makes things easier in this case: the application environment. The application environment is a key-value in-memory global store that’s usually used for things like configuration. We can store the weather API module that we’re using in the application environment and read it when executing rain?/2.

integration_tests/soggy_waffle/soggy_waffle_module_from_app_env.ex

```
def rain?(city, datetime) do
  weather_api_module =
```