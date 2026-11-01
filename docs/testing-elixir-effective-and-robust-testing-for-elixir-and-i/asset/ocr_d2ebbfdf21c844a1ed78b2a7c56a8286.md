In our case, we know that more than one test will likely make use of those lists of IDs, so we’ll add them as module attributes to the top of the test file.

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs

@thunderstorm_ids {
  "thunderstorm",
  [200, 201, 202, 210, 211, 212, 221, 230, 231, 232]
}
@drizzle_ids {"drizzle", [300, 301, 302, 310, 311, 312, 313, 314, 321]}
@rain_ids {"rain", [500, 501, 502, 503, 504, 511, 520, 521, 522, 531]}
```

# Leveraging List Comprehensions

Are you ready for something wild? We’re going to wrap our new test in a list comprehension. Add the following code to your test file and we’ll step through it. Keep the first for at the same level of indentation as the test before it.

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs

1: for {condition, ids} <- [@thunderstorm_ids, @drizzle_ids, @rain_ids] do
  - test "success: recognizes #{condition} as a rainy condition" do
  - now_unix = DateTime.utc_now() |> DateTime.to_unix()
  -
5: for id <- unquote(ids) do
  - record = %{"dt" => now_unix, "weather" => [%{"id" => id}]}
  -
  - assert {:ok, [weather_struct]} =
  - ResponseParser.parse_response(%{"list" => [record]})

10:
  - assert weather_struct.rain? == true
  - end
  - end
  - end
```