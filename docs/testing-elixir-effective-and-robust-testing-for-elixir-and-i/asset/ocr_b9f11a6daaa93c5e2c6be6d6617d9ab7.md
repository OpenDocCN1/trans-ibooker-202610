```
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

The application environment is a global storage; so if we take this approach, we can’t make the test case asynchronous and we have to remember to set the value of :weather_api_module in the application environment back to the original value after the tests.

Before taking a step back and discussing the benefits of dependency doubles in general, let’s look at one last pattern to fix a small code smell that we introduced into the code.

# Improving Performance at Compile Time

The patterns we discussed are powerful and flexible, and they let us have fine-grained control over testing interactions with external services. However, there’s still one principle we’re going against: we’re introducing a runtime call to decide which double to use in production code. That is, every time we’ll call the rain?/2 function, we’ll call Application.get_env/3 (which is essentially a read from an ETS table) to get the double module we want to use. Changing production code to accommodate for testing is often a necessary evil, but it should be limited as much as possible, especially when it comes with a performance cost. In this case, the performance hit is negligible compared to the cost of the HTTP call to the API when running in production. However, you can imagine cases where you might want to