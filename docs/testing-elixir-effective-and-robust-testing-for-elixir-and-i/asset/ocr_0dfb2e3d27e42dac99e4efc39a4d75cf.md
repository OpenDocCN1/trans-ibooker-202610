```
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