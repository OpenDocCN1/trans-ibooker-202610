While list comprehensions allow us to cover more test cases with less test code, they run the risk of obfuscating your errors. If your test fails but you can’t tell which values it was asserting against, you’ll lose time having to debug the failure. In the test we just wrote, how would we know which ID we were testing when we had the wrong value of rain? Let’s update that assertion to use a custom failure message:

```
unit_tests/soggy_waffle_examples/test/soggy_waffle/weather_api/response_parser_test.exs

1: for {condition, ids} <- [@thunderstorm_ids, @drizzle_ids, @rain_ids] do
2:   test "success: recognizes #{condition} as a rainy condition" do
3:     # test body
4:
5:     assert weather_struct.rain? == true,
6:       "Expected weather id (#{id}) to be a rain condition"
7:   end
8: end
```

Adding a custom error message with string interpolation (line 6) now gives us everything we need to know if the test fails. Having this information is very important because otherwise it’ll be impossible to see which specific code caused the test to fail. When designing your tests, take time to make sure that the failure message points you straight at the issue. It doesn’t take long, and you, or your teammates, will thank you later.

+   1) test parse_response/1 success:
+      recognizes drizzle as a rainy condition
+      (SoggyWaffle.WeatherAPI.ResponseParserTest)
+      test/soggy_waffle/weather_api/response_parser_test.exs:32
+      » Expected weather id (300) to be a rain condition
+      code: for id <- unquote(ids) do

# Covering All the Use Cases

While we will discuss property-based testing in a later chapter, we often don’t need anything outside of ExUnit and Elixir to cover the rest of our