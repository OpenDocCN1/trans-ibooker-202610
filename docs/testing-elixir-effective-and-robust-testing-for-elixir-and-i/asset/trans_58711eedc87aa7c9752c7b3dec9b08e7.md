
database transaction, behaving like a database would when running in production. Our interface (API) tests are integration-style, and there’s a good chance that many processes will want to access the same data, for example the user we seeded in the database. You’ll notice that there’s a conditional (unless) wrapped around the line setting the mode. Remember the async: false from the use line of our test file? This code evaluates the value passed for async. This unless block makes it so that any test using NotSkullWeb.ConnCase will default the sandbox mode to shared mode unless it’s overridden with async: true. We added async: false to our test file because being explicit is always good.

Also in that function is a call to the library Mox, on line 2, which we discussed in Chapter 2, Integration and End-to-End Tests. This is a good reminder that the setup block in a test case will be shared by all tests that use that file (unless specifically overridden). In this case, setting Mox to a specific mode doesn’t hurt the tests that don’t use Mox, so while it doesn’t pertain to the tests we’re looking at, it’s not a problem to have it there, either.

The setup block returns a new Plug.Conn because it’s assumed that all the tests using the file will need it.

## 定义错误测试

It’s common to have your success tests before your error tests, but that doesn’t mean you have to write them in that order. In our case, we’re going to start with the simplest test for this endpoint: our error case.

Add the following test inside of the describe block you already created:

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: test "error: does not update, returns errors when given invalid attributes",
   %{
     conn_with_token: conn_with_token,
     user: existing_user
5:   } do
   conn =
   put(conn_with_token, "/api/users/#{existing_user.id}", %{name: ""})
   
   assert body = json_response(conn, 422)

10:

   user_from_db = Repo.get(User, existing_user.id)
   assert user_from_db == existing_user
   
   actual_errors = body["errors"]
refute Enum.empty?(actual_errors)

15:

   
   expected_error_keys = ["field", "message"]
   
   for error <- actual_errors do
    assert_unordered_lists_are_equal(

20:

   actual: Map.keys(error),
   expected: expected_error_keys
   )
   end
  end
25:
```

As mentioned before, the test accepts a context from the setup block, in this case giving it a user and a Plug.Conn that contains a header with a JWT for that user (line 3). Because the setup is done before the test, the very first line of the test body is the call to exercise (line 7). It’s using the function put/3, which it has imported from Phoenix.ConnTest by “using” NotSkullWeb.ConnCase. Similar functions are available for each of the HTTP action types, some of which we’ll use later. In this case, the function takes a Plug.Conn (we’re passing the one with the JWT), the

path for the endpoint, and a PUT body. The return value from all of them is an updated Plug.Conn that contains the response data.

Line 9 uses json_response/2 to do two things: it checks that the HTTP status code on the returned Plug.Conn matches the code that’s passed as the second argument (we’re expecting a 422 in this case). If those values match, it parses the body field of the Plug.Conn from JSON into an Elixir map. While normally it’s not great to do two things at once, especially two unrelated things like an assertion and a transformation, this function has been around for years and has never caused any issues. When it fails, it provides errors that make it clear if it’s failing because of the status code or because it can’t parse the body to JSON.

The rest of the test is fairly straightforward. It checks that, because this is an error case, the data in the database hasn’t been changed as a result of the HTTP call (line 12). This is an assertion that’s often overlooked in error cases but needs to be there. Because the input was invalid, nothing should have been changed in the database. This is an extra check on top of the assertions on the return value. Unlike in most unit tests, endpoint tests, because they’re inherently integration tests, almost always includes assertions for the code under test’s return value and assertions on its side effects.

Speaking of return values, the test also verifies that the response body has errors that are maps with the keys "field" and "message" (line 19). The test does NOT check for specific errors, just that they are shaped correctly. We’ve already written tests to make sure that the correct error is handed back from our database code. This test is just focused on the controller and making sure that it calls out to the right code and can properly handle an error. Since our application standardizes the way we handle errors, we only need one somewhat generic test to confirm that our controller can handle any error correctly.

We are going to add three more tests, but we won’t dive as deeply into what’s happening in each of them, as they build on the basic structure that we just wrote.

## 测试成功路径

Our next test will be our happy path. In the same describe block, add this test, ideally before the error case that we just reviewed:

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: test "success: updates db returns record with good params", %{
  conn_with_token: conn_with_token,
  user: existing_user
} do
5: new_name = "#{existing_user.name}-updated"

conn =
  put(conn_with_token, "/api/users/#{existing_user.id}", %{
    name: new_name
  })

10:

  assert parsed_return = json_response(conn, 200)

  user_from_db = Repo.get(User, existing_user.id)

15:

  assert_values_for(
    expected: %{existing_user | name: new_name},
    actual: user_from_db,
    fields: fields_for(User) -- [:updated_at]
  )

20:

  assert DateTime.to_unix(user_from_db.updated_at, :microsecond) >
    DateTime.to_unix(existing_user.updated_at, :microsecond)

# checking that the updated record is what is returned from endpoint

assert_values_for(
  expected: user_from_db,
  actual: {parsed_return, :string_keys},
  fields: fields_for(User),
  opts: [convert_dates: true]
)
end
```

The test is structured very similarly to the error test we wrote before it.
The common elements are these:

- It accepts a context with a Plug.Conn that contains a valid JWT.
- It calls the endpoint using put/3, a helper from the Phoenix library.
- It uses json_response/2 to assert on the HTTP response code and to deserialize the JSON response.
- It makes assertions on the return value.
- It makes assertions on the side effects.

In this case, it differs from the error case at the assertions. Because it’s a happy path test, we should see that the HTTP response code was a 200. The return value should be a serialized version of the updated record.

The assertions on the return value and the side effect are very related here. Assuming the code under test is working correctly, the return value should be the same information that’s in the row in the database: the updated user. We also want to assert that the return value is the original record updated with the new params, in this case just the new

name. But it’s important to lock down that the only values that changed are the ones that were passed in through the endpoint. To assert that the values are correct for both the return value and the data in the database (the side effect), we’ll turn to a custom helper function, called assert_values_for/1.

## 我们的自定义测试助手

All of the code for the helper function is provided in code/testing_phoenix/not_skull/test/support/assertion_helpers.ex, and you have access to it in the tests you are writing via use NotSkullWeb.ConnCase. It’s more useful here to discuss *what* it does and *why* it does it than how. The function accepts two pieces of data for which Enumerable is implemented. It also takes a list of field names to check against. Assuming atom-based keys, but allowing for the option of string keys, it iterates through and checks that every field named has the same value in both the *expected* and the *actual* data. It also accepts an option to convert between Elixir DateTime structs and ISO 8601 strings. The *why* of this function is simple: most engineers don’t explicitly test all of the data available in their tests because writing assertions on every field is tedious. By creating an easy-to-use, easy-to-reason-around, thorough helper function, we’re attempting to eliminate this bad habit.

The first use of assert_values_for/1 is on line 16, where we define the expected to be the existing user’s values updated with the new name (from the parameters passed in the exercise call). A second helper function called fields_for/1 is used on line 19. It returns a list of all the fields for whatever you pass it. In our case, we’re giving it the User struct. Because it’s a list, we can easily remove any fields we want to skip by using Elixir low-level list subtraction, Kernel.--/2. In our case, we’re removing :updated_at from the list of values to check because it’ll have changed and we don’t have a way to know the new value from inside our test. We’ve tested enough of the fields to have confidence that our code called the correct function(s) under the hood. Because we emphasize thoroughness, though, we’ve added one last assertion to
