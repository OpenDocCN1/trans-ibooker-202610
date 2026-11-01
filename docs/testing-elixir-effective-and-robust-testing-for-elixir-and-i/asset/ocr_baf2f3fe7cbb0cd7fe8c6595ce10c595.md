```
assert meta[:validation] == :cast,
  "The validation type, #{meta[:validation]}, is incorrect."
end
end
```

The only actual changes are on the line that exercises the code (4), but there are a few different things on that line. First, we need to update the pattern on the left of the = to wrap the changeset in a tuple with :error as the first element. Our test no longer cares if it’s getting an invalid changeset back; instead, it only cares that if it gets an error tuple, the second element is a changeset with errors. So, we can remove the valid?: false from the pattern. Finally, update the function being called from changeset/1 to cast_and_validate/1. The test should run and pass just fine now.

The last test should take the same changes and then look like this:

```
test "error: returns error changeset when required fields are missing" do
  params = %{}

  assert {:error, %Changeset{errors: errors}} =
    UserValidator.cast_and_validate(params)

  for {field, _} <- @expected_fields_with_types, field not in @optional do
    assert errors[field], "The field :#{field} is missing from errors."
    {_, meta} = errors[field]

    assert meta[:validation] == :required,
      "The validation type, #{meta[:validation]}, is incorrect."
  end
end
```

All the tests should now be passing. You can see that the main concepts of our tests stay the same as when we were testing our basic schema. The tests just had to be updated to reflect the small changes in the UserValidator's interface.

Next we’ll step back to the basic schema and modify it for interactions with the database. Then we’ll discuss the ways to test a database