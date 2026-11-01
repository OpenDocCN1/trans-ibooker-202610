we get different values for the new field. This isn’t strictly necessary, but it makes our data slightly more realistic. We’re using a new library, Faker, to provide some of that data. Faker’s sole purpose is to provide realistically shaped data. In our case, we’re using the module Lorem to generate random words. In order to keep our code compiling and running, you’ll need to add Faker into the project dependencies. In the deps section of the mix.exs file, you’ll need to add the line {:faker, "~> 0.13.0", only: [:test, :dev]}. We’ve limited it to the test and dev environments because we don’t intend to use Faker in production code. We’ve left it in :dev because it’s nice to be able to drop into a shell session (iex -S mix) to explore the functionality.

Now that we have our helper function, we’re ready to write our test. Add this new describe block and test to your test file. You’ll notice that it’s almost the same, except that our list comprehension is using the @fields_and_types for its generator (but dropping the type) and that the valid parameters are coming from the helper function instead of being hard-coded.

```
```elixir
describe "changeset/1" do
  test "success: returns a valid changeset when given valid arguments" do
    valid_params = valid_params(@expected_fields_with_types)

    changeset = UserBasicSchema.changeset(valid_params)
    assert %Changeset{valid?: true, changes: changes} = changeset

    mutated = [:date_of_birth]

    for {field, _} <- @expected_fields_with_types, field not in mutated do
      actual = Map.get(changes, field)
      expected = valid_params[Atom.to_string(field)]
      assert actual == expected,
        "Values did not match for field: #{field}\n expected: #{inspect(expected)}\nactual: #{inspect(actual)}"
    end

    expected_dob = Date.from_iso8601!(valid_params["date_of_birth"])
```
```