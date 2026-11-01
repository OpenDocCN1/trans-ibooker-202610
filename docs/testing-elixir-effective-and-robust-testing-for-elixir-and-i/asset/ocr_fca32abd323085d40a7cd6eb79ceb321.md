```
1: test "error: returns error changeset when an email address is reused" do
-   {:ok, existing_user} =
-   valid_params(@expected_fields_with_types)
-   |> UserDatabaseSchema.changeset()
5:   |> TestingEcto.Repo.insert()
- 
-   changeset_with_repeated_email =
-   valid_params(@expected_fields_with_types)
-   |> Map.put("email", existing_user.email)
|> UserDatabaseSchema.changeset()
10: 
- 
-   assert {:error, %Changeset{valid?: false, errors: errors}} =
-       TestingEcto.Repo.insert(changeset_with_repeated_email)
- 
    assert errors[:email], "The field :email is missing from errors."
15: 
-   {_, meta} = errors[:email]
- 
-   assert meta[:constraint] == :unique,
-       "The validation type, #{meta[:validation]}, is incorrect."
    end
20:
```

Let’s examine this test and see how it differs from all our previous testing. Our setup is creating an existing user in the database, actually calling TestingEcto.Repo.insert/1 at line 5. We need this insert because we’re trying to trigger an error in the database for a unique constraint violation.

The second part of the setup is to create a changeset with valid params except for the email address, which we’re overriding with the email address from the existing user (line 9). The exercise step of this test is not normal. The code under test, the changeset/1 function, has already been called. Instead, we’re using a second insert call to the database to trigger the error we’re looking for. You can see that