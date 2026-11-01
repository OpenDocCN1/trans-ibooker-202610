# Creating Comprehensive Test Coverage

We’ve covered some of the very basics of testing and delved into a bit of test theory. In order to provide solid coverage of a file, we’ll need to build on that knowledge by pulling in some features of Elixir itself, as well as some optional features of ExUnit’s assertions. These skills will help you write maximum test coverage with minimal code and without trade-offs.

# Using a Setup Block

Let’s jump back to where we left our Soggy Waffle test, SoggyWaffle.WeatherAPI.ResponseParserTest. We’ll make a modification to our existing test before adding some more tests to our response parser. The data we used in our test was hand-coded. That’s a great way to start, but we don’t own the original source of the data, the weather API. While it’s easy to read the hard-coded data, it’s actually a significantly pared down version of the real data from the weather API. It would be nice to be working with data that’s as realistic as possible. To do this, we can use a fixture file. A simple curl call to the API can provide an actual JSON response payload. We’ve saved that to a file in the application, test/support/weather_api_response.json. Let’s modify our test to use that data instead of the handwritten values we used earlier:

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs

1: describe "parse_response/1" do
- setup_all do
- response_as_string =
- File.read!("test/support/weather_api_response.json")
5:
- response_as_map = Jason.decode!(response_as_string)
- %{weather_data: response_as_map}
- end
```