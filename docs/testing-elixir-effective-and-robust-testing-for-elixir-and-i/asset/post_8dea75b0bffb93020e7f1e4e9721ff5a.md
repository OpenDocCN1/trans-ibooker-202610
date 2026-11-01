
to make changes to the schema, the tests we wrote would either need to be updated (if new fields are added) or supplemented with new tests (if new validations are added). Later in this chapter, we'll branch the code and tests we just wrote to specialize our schema for either input validation or for database interactions. Before we do that, though, we're going to make another pass at the tests we just wrote to identify patterns and changes we can make to specialize our tests for code that uses Ecto.Schema.

## Refactoring to Increase Test Maintainability

Most applications that use Ecto have more than one schema, and over time each schema grows to have more fields or more logic in its changeset functions. We’re going to refactor our existing test file to allow our code to be easier to maintain, but that means we’re going to tread into some territory that makes some folk uncomfortable: we’re going to refactor our tests to be self-updating. We’ll build safety into the way we test to avoid a lot of the typical caveats that leave most people burned by writing self-updating tests.

We want to make it clear that we don’t promote this style of testing wholesale across your application. We’re working in a section of our code that’s fairly predictable and limited in scope. Our schemas only carry the definition and changeset functions.

We’re going to create a new test file, testing_ecto/test/schemas/user_basic_schema_2_test.exs, but we’re going to have it test the same schema code as our previous test file. We mentioned earlier that we were adding numbers into the test file names that would break the convention of matching the test and application file names. Even though we called this a refactor, it’s probably best to create a second file, too, because doing so will make it easier to follow along with the book.

## Adding Safety Into Your Schema Tests

Open your new test file and add the following code. You’ll see that we’re testing the same module as our previous file, TestingEcto.Schemas.UserBasicSchema:

```
testing_ecto/test/schemas/user_basic_schema_2_test.exs
```

```
defmodule TestingEcto.Schemas.UserBasicSchema2Test do
  use ExUnit.Case
```

```elixir
alias Ecto.Changeset
alias TestingEcto.Schemas.UserBasicSchema

@expected_fields_with_types [
  {:date_of_birth, :date},
  {:email, :string},
  {:favorite_number, :float},
  {:first_name, :string},
  {:last_name, :string},
  {:phone_number, :string}
]
describe "fields and types" do
  @tag :schema_definition

  test "it has the correct fields and types" do
    actual_fields_with_types =
      for field <- UserBasicSchema.__schema__(:fields) do
        type = UserBasicSchema.__schema__(:type, field)
        {field, type}
      end

    assert MapSet.new(actual_fields_with_types) ==
      MapSet.new(@expected_fields_with_types)
  end
end
end
```

Aside from the module name, the first real change is that instead of having a module attribute, @schema_fields, we have an attribute called @expected_fields_with_types. This attribute contains a keyword list with the field names and the expected field type for our schema. Our previous test did give us confidence that the schema defined the correct types for the field, but having this information duplicated in the test file is a

major component of the safety we promised to give you for this refactor. We’ll come back to why it’s important after we’ve added more to our file.

The describe block at line 14 doesn’t follow the convention of a function name because we aren’t testing a public function. Instead, our exercise code is a little unusual. Instead of calling a function, we’re building a keyword list of field names and types similar to that in our @expected_fields_with_types module attribute. To do this, we’re leveraging two calls to reflection functions from Ecto.Schema. If you recall, we first saw reflection in the schema file itself. We’re already familiar with __schema__/1 (line 18). __schema__/2 (line 19) returns a single atom for the field type. Our generator is just a call to UserBasicSchema.__schema__(:fields). Each iteration then calls UserBasicSchema.__schema__(:type, field), which returns an atom representing the type, which will be exactly what was set in the schema file itself. Our list comprehension returns a keyword list of field names and types that comes from the schema definition itself, but it’s formatted in the same way as the data stored in @expected_fields_with_types.

This test then checks that the test’s expectation of the field names and types (@expected_fields_with_types) matches what’s really defined in the file. Again, we didn’t do this in our first pass on the file because we didn’t need to. Our tests provided coverage for this without us being so explicit. It’s important to note that since we’re using == for the comparison in our assertion, both lists must be in the same order for the assertion to pass. To address that, we call MapSet.new/1 on both lists. Sets are unordered, and comparing two sets only takes elements into consideration, making it perfect for this use case.

One last new feature of this test is the addition of a test tag, :schema_definition, at line 15. At this point, our tests haven’t evolved

enough for this to be useful. Later in this chapter, we’ll leverage this tag to only run this test.

We stated that this test adds safety to the rest of the refactoring work we’re about to do. Let’s play with this for a moment. Comment out one of the fields from your schema; let’s say field(:first_name, :string). Now run your tests and check the output. It should look something like the following.

```
1) test fields and types it has the correct fields and types
(TestingEcto.Schemas.UserBasicSchema2Test)
test/schemas/user_basic_schema_2_test.exs:16
Assertion with == failed
code: assert MapSet.new(actual_fields_with_types) ==
    MapSet.new(@expected_fields_with_types)
left: #MapSet<[{:date_of_birth, :date}, {:favorite_number, :float},
    {:last_name, :string}, {:phone_number, :string}]>
right: #MapSet<[{:date_of_birth, :date}, {:favorite_number, :float},
    {:first_name, :string}, {:last_name, :string},
    {:phone_number, :string}]>
stacktrace:
    test/schemas/user_basic_schema_2_test.exs:23: (test))
```

If you have color output from your tests, you’ll see immediately that {:first_name, :string} is extra in the expected fields. A failure similar to this would happen if you were to add a new field to your schema. We now have very fast validation feedback that something isn’t right, which points us back to the schema definition. @expected_fields_with_types is also going to be used in our upcoming tests as a reference for how the schema is defined. The test we just wrote will help us make sure it’s always accurate. If you need to add or remove fields from your schema, you just need to update the list stored in @expected_fields_with_types as well.

There’s an added bonus to having this definition at the top of our test file. We’ve mentioned previously in this book that well-written tests serve as documentation for your code. In this case, we’ve taken it a

step further by making sure there’s an accurate description of the schema basics sitting at the top of the file. We don’t recommend doing this solely for the sake of having extra reference information in your tests, but it can be nice to have when working in the test file.

Now that we have guaranteed its accuracy, let’s write a test to take advantage of @expected_fields_with_types. Before we continue, uncomment the field for first name to get our code back to the correct state.

## Creating a Self-Updating Test

Before we write our first test, we’re going to write a helper function at the bottom of our test file, after all describe blocks. The new function will be called valid_params/1. Define it with the following code:

```
defp valid_params(fields_with_types) do
  valid_value_by_type = %{
    date: fn -> to_string(Faker.Date.date_of_birth()) end,
    float: fn -> :rand.uniform() * 10 end,
    string: fn -> Faker.Lorem.word() end
  }

  for {field, type} <- fields_with_types, into: %{} do
    {Atom.to_string(field), valid_value_by_type[type].()}
  end
end
```

We’ll address the introduction of Faker shortly, but let’s discuss what this function does first. The return value of this function is a map functionally identical to the params we had in our success test in our previous iteration of this file. It’s just a string-keyed map of valid parameters, but it’s built dynamically based off of the list of fields and types passed to it. We’ll evolve this concept later on. For now, we have a way to get a list of valid parameters in a repeatable way. The values provided are randomized because the values themselves shouldn’t impact the results of the test. We’re using anonymous functions so that
