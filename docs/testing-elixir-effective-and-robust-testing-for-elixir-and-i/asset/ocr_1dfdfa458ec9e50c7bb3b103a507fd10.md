# Testing an Ecto Schema as a Data Validator

It’s very common in web applications to do some basic validation of user input in the controller so that if easy-to-detect issues are present, the application doesn’t spend time doing work (or calling the database) when there isn’t a chance of success. The code that we have already written takes input and casts it to the appropriate value, returning errors for any data that can’t be cast or for any required parameters that are missing. This is exactly the kind of basic validation that we’re talking about. With some very minor changes, we will update our code to serve as a validator. After that, we will update our refactored tests to reflect the changes.

As mentioned, our code already does the work we want. The only issues we have now is that the interface isn’t ideal. Our application code isn’t going to want to receive a changeset when the code executes successfully. Ideally, when the input is valid, our validator will return a tuple with :ok and a struct with the fields and values. When it errors, we’ll still return the changeset with errors, but inside of an error tuple. There’s one other interface change we should make: renaming the function. Since we’re no longer returning a changeset, we’ll rename the one public function, changeset/1, to instead reflect what the code is doing. Make a copy of the schema file at the path lib/schemas/user_validator.ex. We’ll show the updated file and then call out the changes that need to be made:

```
testing_ecto/lib/schemas/user_validator.ex

1: defmodule TestingEcto.Schemas.UserValidator do
  - use Ecto.Schema
  - import Ecto.Changeset
  -
5: @optional_fields [:favorite_number]
  -
  - @primary_key false
```