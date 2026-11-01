```
20:
-   assert meta[:validation] == :cast,
-       "The validation type, #{meta[:validation]}, is incorrect."
-   end
-   end
```

The setup is similar to the success test, but you'll see that we've found different data types that we know Ecto can't cast to the types defined in the schema. Our line to exercise is the same, and the following pattern match and assertion only differ in that they expect valid? to be false. But then we hit a different list comprehension. Using our list of expected fields, @schema_fields, we have a two-part assertion for each field. The first, at line 18, is to make sure there's an error for that field. The second (line 22) is to make sure it's a cast error. When you write assertions like this, it's helpful to make sure your custom error messages are different. Even though test output tells you the line where the error occurred, having the same message will make it too easy to accidentally debug a failure at the wrong assertion. Ultimately, we only care that a cast error is present, but because there are slight differences in the errors for each field, as shown in the following code sample, we have to jump through a couple of hoops. The following snippet shows the errors from our changeset:

```
[
  favorite_number: {"is invalid", [type: :float, validation: :cast]},
  date_of_birth: {"is invalid", [type: :date, validation: :cast]},
  last_name: {"is invalid", [type: :string, validation: :cast]},
  phone_number: {"is invalid", [type: :string, validation: :cast]}
]
```

Each error has a different type, which causes the need for the multistep assertions. Another approach to this would be to process the errors ahead of time and remap the list to only contain the field name and the validation type. Phoenix provides a function, called errors_on, that remaps errors to just be the field name and the error message (the