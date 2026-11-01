```
assert body = json_response(conn, 403)

assert %{
    "errors" => [
    %{
        "message" => "You are not authorized for that action.",
        "field" => "token"
    }
    ]
} == body

user_from_db = Repo.get(User, existing_user.id)

assert_values_for(
    expected: existing_user,
    actual: user_from_db,
    fields: fields_for(User)
)
end
end
end
```

The test itself is structured like our other Plug test. The differences are that it uses the Plug.Conn with the valid token (JWT) and that the user ID in the URL is different from what’s encoded in the token. The assertions are similar, checking the status code (this time a 403), checking the error(s) in the response body, and finally asserting that there were no side effects.

By running through these tests, we’ve laid out a basic structure that will be used or built on in our tests for the other kinds of application interfaces. Let’s look at server-rendered HTML endpoints next.