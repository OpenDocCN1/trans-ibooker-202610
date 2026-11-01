# Testing Delete

Our last function, delete/1, is simple to test, as the code is a straight pass-through to Repo.delete/1. Let’s look at the function:

```
testing_ecto/lib/users/users.ex

def delete(%User{} = user) do
  Repo.delete(user)
end
end
```

We’ll only test the success path for this function because errors are incredibly unlikely. Additionally, writing a test that can force an error on delete is complicated and requires restructuring our code solely for testing. In this case, the payoff isn’t there. When that’s the case, we typically let our applications crash instead of trying to handle every possible failure. Let’s write our final test and then review what it does:

```
describe "delete/1" do
  test "success: it deletes the user" do
    user = Factory.insert(:user)

    assert {:ok, _deleted_user} = Users.delete(user)

    refute Repo.get(User, user.id)
  end
end
```

Our test sets up by inserting a user. It then exercises the delete/1 function, asserting that it returns a success tuple. The last line is an assertion that the data is no longer in the database. In our case, we aren’t even concerned about the return value, so we’re dropping it. The most important thing is to just make sure that you have that last check on the database.