our test run. A result of 0% means that none of the code in that module has been executed during the test run.

The built-in code coverage tool also generates a bunch of HTML report files (by default in the cover directory). Let’s open the HTML file for the SoggyWaffle.Weather module, you can see what it looks like in the figure.

```
cover/Elixir.SoggyWaffle.Weather.html

1  defmodule SoggyWaffle.Weather do
2      @type t :: %__MODULE__{}
3
4  4  defstruct [:datetime, :rain?]
5
6
7      @spec imminent_rain?([t()], DateTime.t()) :: boolean()
8  :-(  def imminent_rain?(weather_data, now \\ DateTime.utc_now()) do
9      5  Enum.any?(weather_data, fn
10         %__MODULE__{rain?: true} = weather ->
11      6  in_next_4_hours?(now, weather.datetime)
12
13         _ ->
14         false
15     end)
16     end
17
18     defp in_next_4_hours?(now, weather_datetime) do
19      6  four_hours_from_now =
20         DateTime.add(now, _4_hours_in_seconds = 4 * 60 * 60)
21
22      6  DateTime.compare(weather_datetime, now) in [:gt, :eq] and
23      5  DateTime.compare(weather_datetime, four_hours_from_now) in [:lt, :eq]
24     end
    end
```

Reading this report is straightforward. The lines highlighted in green are the ones that’ve been executed during the test run. Each of the green lines has a number to the left that shows how many times the line has been executed. The lines highlighted in red are the ones that have not been executed during the test run. All the other lines are lines that don’t account for any coverage for various reasons. For