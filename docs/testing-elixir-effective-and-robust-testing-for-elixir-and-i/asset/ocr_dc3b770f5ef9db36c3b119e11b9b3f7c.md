```
assert changes.date_of_birth == expected_dob
end

end
```

Now that you’ve added the test, run the test file to make sure it’s passing. Once you’ve got it passing, let’s explore the benefit of this refactor. Think of the first test, asserting on fields and types, as a control. Any changes you make to your file will require an intentional update to that control. So, let’s imagine that our code is being upgraded and that we’re going to add a new field to our schema. Add a new middle name field (`field(:middle_name, :string)`) into the schema definition. Now run your tests again, using `mix test` `test/schemas/user_basic_schema_2_test.exs`. You’ll see more than one test failing.

While it’s easy to pick out the test failure to focus on when you have a fairly new and small test file, you may have more test failures in a scenario like this as your schema grows. Remember that tag we added to the test? Since we know we changed the fields on the schema, it seems like a good time to isolate the test that focuses on the schema definitions. Rerun your tests, but this time we’ll pass the tag as well, `mix test --only schema_definition test/schemas/user_basic_schema_2_test.exs`. You should see a single failure similar to the following:

```
1) test fields and types it has the correct fields and types
    (TestingEcto.Schemas.UserBasicSchema2Test)
    test/schemas/user_basic_schema_2_test.exs:16
    Assertion with == failed
    code: assert Enum.sort(actual_fields_with_types) ==
        Enum.sort(@expected_fields_with_types)
    left: [{:date_of_birth, :date}, {:favorite_number, :float},
        {:first_name, :string}, {:last_name, :string},
        {:middle_name, :string}, {:phone_number, :string}]
    right: [date_of_birth: :date, favorite_number: :float,
        first_name: :string, last_name: :string, phone_number: :string]
    stacktrace:
        test/schemas/user_basic_schema_2_test.exs:23: (test)
```