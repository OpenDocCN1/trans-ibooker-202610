shave off even the handful of microseconds it takes to read from the ETS table in Application.get_env/3.

To solve this problem, a likely initial approach is to read the module at compile time using Application.compile_env/3. compile_env/3 works like get_env/3, but this function enforces that it’s used at compile time and allows Elixir to track configuration values that change between runtime and compile time. We won’t focus on this feature in particular here, but it’s good practice to use compile_env/3 in cases such as this one.

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

This approach works well in a few cases, but it has a critical drawback: you can only use one double module throughout your testing environment. The reason for this is that we read the application environment at compile time, so our code will be compiled with a specific double module as the value for @weather_api_module. What this all means is that you won’t be able to test both the actual SoggyWaffle.WeatherAPI module and use doubles to isolate tests like we discussed in this chapter.