# Introducing Randomness and Property-Based Testing

We can solve some of the problems that example-based tests suffer from by introducing a bit of chaos in our tests. Using randomness to generate inputs will allow us to test a wider range of inputs against our code and potentially create inputs that trigger edge cases.

In our sorting example, what we really want to test is that the output of Enum.sort/1 is a sorted version of the input. For any random input, we can think of a few properties that the output will always retain. For example, the output list always has the same length as the input list. Another property is that the output list is always sorted, which is something that we can check in a pretty straightforward way by checking that each element is smaller than or equal to the following one. Now that we’ve thought of these properties, we could change our test so that we generate random lists and test these properties on the output of our code instead of checking what the output is. Let’s see how to do that:

```
property_based_testing/sorting/test/randomized_sort_test.exs

```elixir
1: defmodule RandomizedSortTest do
  use ExUnit.Case

  test "Enum.sort/1 sorts lists" do
5:   for _ <- 1..10 do
      random_list = random_list()
      sorted_list = Enum.sort(random_list)

      assert length(random_list) == length(sorted_list)
      assert sorted?(sorted_list)
10:
    end
  end
```
```