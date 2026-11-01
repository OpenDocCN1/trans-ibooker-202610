In the router (code/testing_phoenix/not_skull/lib/not_skull_web/router.ex), our update endpoint is in a pipeline that contains our two custom plugs: ValidateJWT and MatchJWTUserId. Let's take a look at each, understanding what it does and then adding a test to make sure it's present.

The first Plug, ValidateJWT, decodes the JWT and makes sure that it's valid. If it is valid, the Plug takes the user's ID from the JWT and adds it into the incoming params. If the Plug doesn't detect a valid JWT, it halts the call stack, returning an HTTP status code of 401 ("Unauthenticated") and a simple JSON response with a descriptive error.

Add the following new test in the same describe block, "PUT /api/users/:id", after your error test:

```
testing_phoenix/not_skull/test/not_skull_web/controllers/json_api/user_controller_test.exs

1: test "auth error: returns 401 when valid jwt isn't in headers", %{
  -   conn: conn,
  -   user: existing_user
  - } do
5:   conn =
  -   put(conn, "/api/users/#{existing_user.id}", %{
  -     name: "#{existing_user.name}-updated"
  -   })
  - 
    assert body = json_response(conn, 401)
10:
  - 
  -   assert %{
  -     "errors" => [
  -     %{"message" => "Invalid token.", "field" => "token"}
    ]
15:
  -   } == body
  -
```