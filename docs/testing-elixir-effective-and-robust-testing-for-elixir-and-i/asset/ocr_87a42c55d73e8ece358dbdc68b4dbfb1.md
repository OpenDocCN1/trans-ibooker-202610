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
```

When starting a new Phoenix project, the generators will create a few different ExUnit test cases. One of them, ConnCase, or specifically NotSkull.ConnCase in our app, will be used as the test case for all the HTTP-based interfaces. It’s pulled in, as seen on line 2, by “using” the module. This makes a number of helpers available, making it easier for us to build and manipulate conns, or Plug.Conn structs. These helpers are a main extra component when testing web interfaces. Additionally, using the test case also imports our own custom functions, defined in NotSkullWeb.ConnCase itself. This allows us to create functions that we need more than once, across different tests, and to make them available in the tests that need them.

The string passed to describe breaks our pattern of using a function name and arity because, as a controller test, the exercise will hit an endpoint instead of calling a specific function. As a result, we’ve named the describe block after the HTTP action and path: "PUT /api/users/:id".

# Leveraging the ConnCase