
## Testing JSON-Based APIs

We’re going to start by looking at testing a JSON-based API. This is a very common use case for Phoenix and it’s the least complex of the interfaces we’ll test. This will allow us to focus on some concepts that will apply across all of the interface types we’ll cover, without getting too lost in the specifics of the interface itself.

## What Controller Tests Need to Cover

We strive to keep our controllers as small as possible, with as little branching logic as possible. This accomplishes two goals: it keeps our business logic out of the web part of our application, and it keeps the number of test cases needed to a minimum. Controller tests are some of our most expensive (in terms of running time) since they often require the most setup. Ideally, we have as few of them as possible while still leaving our application with solid coverage.

The logic for most controller actions can be tested with two tests: a happy path test and an error case test. There will always be exceptions to this, but it should be your starting goal.

Beyond testing the logic in the actions themselves, you may need to include tests to verify that certain plugs are included in your application’s call stack. In this section, we’ll test a JSON-based endpoint, with a happy path and an error case, and we’ll test for the presence of a plug.

Before we start writing tests, let’s look at the code we’ll test and how it fits into the application.

## Familiarizing Ourselves with the User Controller

Our application is a simple game. It’s a pared-down version of a classic game called Skull and Roses. In our version, players simply take turns guessing if the other players have presented a skull or a rose. Our focus starts when players register in the application as users. Users are then able to create a new game or edit their own profile settings. While the application has HTML-rendered controller actions for user registration and profile updates, the JSON-based API provides this same functionality. We’ll focus on testing the edit endpoint, meaning that our test will start with a registered user. Let’s take a look at the controller code called when a user hits the update endpoint to change their own profile:

```
testing_phoenix/not_skull/lib/not_skull_web/controllers/json_api/user_controller.ex

1: defmodule NotSkullWeb.JsonApi.UserController do
-   @moduledoc false
-
-   use NotSkullWeb, :controller
5:
-   alias NotSkull.Accounts
-
-   def update(conn, params) do
-     with {:ok, user} <- Accounts.get_user_by_id(params["id"]),
         {:ok, updated_user} <- Accounts.update_user(user, params) do
10:
-
         conn
         |> put_status(200)
         |> json(user_map_from_struct(updated_user))
     else
         {:error, error_changeset} ->
15:
-
         conn
         |> put_status(422)
         |> json(errors_from_changeset(error_changeset))
     end
end
20:
-
-   defp user_map_from_struct(user) do
-   user
-   |> Map.from_struct()
    |> Map.drop([:__struct__, :__meta__])
25:
-   end
-   
-   defp errors_from_changeset(changeset) do
-   serializable_errors =
      for {field, {message, _}} <- changeset.errors,
30:
-       do: %{"field" => to_string(field), "message" => message}
-   %{errors: serializable_errors}
-   end
```

The start of the file looks like any other controller file. The update function is a standard controller action, accepting a Plug.Conn and a map of params. There’s a two-condition with statement (line 9) that’ll either execute the code in the provided block if successful (line 11) or in the error handling (line 16) if something goes wrong.

Only an existing user can call this endpoint. They must be logged in and they can only try to edit their own information. To enforce this, the application has a minimal implementation of JSON Web Tokens (JWT). [43] Calling the endpoint to update the user will require a signed JWT, gained by logging in through the login controller. The encoded JWT contains the user’s ID, allowing the application to verify that a user is logged in. This decode and validation logic exists in a custom plug (found in lib/not_skull_web/plugs/validate_jwt.ex), which is why it isn’t seen in this file. There’s another plug that makes sure that the user ID in the JWT matches the user ID in the path.

Both of these plugs are unit-tested, which is possible because plugs are purely functional. While we aren’t covering the testing of the plugs used in this chapter, they are tested in the application. The code under test, as well as tests for verifying the JWT, can be found at

lib/not_skull_web/plugs/validate_jwt.ex and test/not_skull_web/plugs/validate_jwt_test.exs, respectively. The code under test can be found in lib/not_skull_web/plugs/match_jwt_user_id.ex, and the tests are in test/not_skull_web/plugs/match_jwt_user_id_test.exs. Aside from needing to conform to the standards for plug callbacks (plugs must have a call/2 function that takes a Plug.Conn and parameters), you can see that the tests are structured like they would be for any purely functional code, only focusing on return values.

While we won’t spend more time on the testing of the plugs, we will need to write tests to make sure they’re utilized for this endpoint. After we’ve written some tests around the logic in the controller itself, we can learn how to test for the presence of plugs.

Let’s starting writing tests.

## Testing the Update Endpoint

In the included repo, NotSkull, create a new test file at test/not_skull_web/controllers/json_api/user_controller_test.exs. We’ve added nesting under json_api to avoid conflicting with the tests for the user controller that we used for the server-rendered HTML endpoint.

The following code will add a basic structure and setup block to the file you created:

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: defmodule NotSkullWeb.JsonApi.UserControllerTest do
  -   use NotSkullWeb.ConnCase, async: false
  -   alias NotSkull.Accounts.User
5:
  -   describe "PUT /api/users/:id" do
  -     setup context do
  -       {:ok, user} = Factory.insert(:user)
```
```elixir
conn_with_token =
  put_req_header(
    context.conn,
    "authorization",
    "Bearer " <> sign_jwt(user.id)
  )

Map.merge(context, %{user: user, conn_with_token: conn_with_token})
end
end
end
```

When starting a new Phoenix project, the generators will create a few different ExUnit test cases. One of them, ConnCase, or specifically NotSkull.ConnCase in our app, will be used as the test case for all the HTTP-based interfaces. It’s pulled in, as seen on line 2, by “using” the module. This makes a number of helpers available, making it easier for us to build and manipulate conns, or Plug.Conn structs. These helpers are a main extra component when testing web interfaces. Additionally, using the test case also imports our own custom functions, defined in NotSkullWeb.ConnCase itself. This allows us to create functions that we need more than once, across different tests, and to make them available in the tests that need them.

The string passed to describe breaks our pattern of using a function name and arity because, as a controller test, the exercise will hit an endpoint instead of calling a specific function. As a result, we’ve named the describe block after the HTTP action and path: "PUT /api/users/:id".

## Leveraging the ConnCase

Our tests will use a common setup block, as most of them need two things: a user to exist in the database and a signed JWT for that user. It’s good to default to having your setup in each test so that the test reads without having to look elsewhere. If you have multiple tests that need the same setup, though, it can be useful to move that common code into a single place, in this case the setup block. The JWT is included as a header in the Conn, as seen on line 10. The actual JWT is created by a function defined in NotSkullWeb.ConnCase (called on line 14), as we need to share some of its logic with the other controller test file, for the login controller. While diving into how the JWT is created isn’t terribly helpful, adding things to request headers with put_req_header (line 11) is a very common thing to do in tests like these. That function comes from Plug.Conn and is imported via NotSkullWeb.ConnCase.

Unlike most of the tests we’ve written in previous chapters, our setup block accepts a context. This context comes from the test case. The generated file has a function that looks like this:

```
testing_phoenix/not_skull/test/support/conn_case.ex

1: setup tags do
  -   Mox.verify_on_exit!()
  - 
  -   :ok = Ecto.Adapters.SQL.Sandbox.checkout(NotSkull.Repo)
5:
  -   unless tags[:async] do
  -     Ecto.Adapters.SQL.Sandbox.mode(NotSkull.Repo, {:shared, self()})
  -   end
  - 
      {:ok, conn: Phoenix.ConnTest.build_conn()}
10:
  -   end
```

The first behavior worth noting is that it sets the sandbox mode to :shared (line 7). This setting is important because it impacts how your tests can be run. Shared mode allows any process to access the same
