When used in the global test context, Mox will verify expectations at the end of each test. If we want more control, we can always use describe blocks (as mentioned in Describing Your Tests) and only use the setup callback in the blocks where we need it.

# Allowing Mocks to Be Used from Different Processes

We haven’t discussed how Mox decides who’s allowed to call the mock or stub functions. When a function of a double is stubbed or mocked, that stub or mock by default only works when that function is invoked from the process that defines the stub or mock. In our weather API example, this doesn’t cause any problems since the weather API interface is used directly from the process that calls rain?/2, which is the test process. However, let’s slightly change the code for the rain?/2 function so that the weather API interface is invoked in a different process. We’ll use Task.async/1 to spawn the new process and Task.await/1 to block until the call to the weather API is done. We’re really forcing the code to use another process since it doesn’t make sense in this particular case to call the API asynchronously, but it’ll help show this Mox functionality, so bear with us for just a second:

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
```