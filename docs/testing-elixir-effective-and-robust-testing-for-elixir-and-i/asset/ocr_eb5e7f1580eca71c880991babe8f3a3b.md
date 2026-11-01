The first line of code executed when the test double is called is send(test_pid, {:get_forecast_called, city}) (seen on line 8). This code sends the value bound to that variable back to the test process itself. At the end of the test, on line 16, the code uses assert_received to verify that the value the code under test passed to the test double was the same value the test originally sent when exercising the code.

Our test uses a naive way to randomize the data, using Enum.random/1 to choose the value passed in and expected to be passed to the double. In an application with more complete data generators, like Factory.city_name/0 or something similar, the data set might be larger, but the effect will be the same: the city name itself is incidental, but the same value must be passed along by the code under test.

Randomizing the incidental data in this test makes sure that the code under test isn't changing that data. Our tests should focus on the outside contracts of the code under test (keeping our black box), and in this case that means input data and data sent to another function or process.

Understanding incidental test data will help us define essential test data. In our previous example, we don't need to test all the possible input values. If a specific set of city names would work while others wouldn't, the data would no longer be considered incidental but instead would be essential. We would need to test that every valid city name did work while names not in that list did not.

An example of this in the book is from Chapter 1, Unit Tests, where we used a list comprehension to assert on every valid response value that we knew of from the weather API we were calling:

```
when_to_randomize_data/response_parser_test.exs
```

```
defmodule SoggyWaffle.WeatherAPI.ResponseParserTest do
  use ExUnit.Case
  alias SoggyWaffle.WeatherAPI.ResponseParser
```