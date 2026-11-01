use ExVCR.Mock, adapter: ExVCR.Adapter.Hackney

ExVCR provides a use_cassette/2 macro that takes a cassette name and a block of code. Requests executed in the block of code are recorded to and replayed from the specified cassette. Let’s rewrite the get_forecast/1 test to use use_cassette/2.

```
integration_tests/soggy_waffle_actual_integrations/test/soggy_waffle/weather_api_t
_t_ex_vcr.exs
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

The first time this test is run, the request is issued against the real weather API. After the weather API returns a response, that response is recorded into a cassette called weather_api_successful_request. We use a descriptive and unique name so that it won’t conflict with other cassettes. When the same test is run again, no HTTP requests are made and the response recorded into the cassette is returned to the HTTP client.

This approach differs from a test server because it focuses less on asserting that the request is made correctly. The main goal of a cassette is to behave exactly like the real third-party service without having to write code to emulate that third-party service. The workflow is, in fact, simpler than the test server: we just wrap our code with use_cassette/2 and go on about our day. However, cassettes present a