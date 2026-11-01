```
) 
# assertions
end

test "some other test" do
  assert {:ok, _pid} =
    RollingAverageServer.start_link(
      name: :my_server,
      max_measurements: 3
    )

  # other assertions
  end
```

In both tests, the server is started with the same name: :my_server option. When ExUnit finishes running one of the tests and starts the other one, there’s no guarantee that the server registered under the :my_server name (started in the previous test) has terminated. Thus, there’s a chance that the second test fails because another server registered with same :my_server name can’t be started.

Lucky for us, ExUnit provides a useful workaround:
ExUnit.Callbacks.start_supervised/1. This function takes a child specification to start your process under an on-the-fly supervisor that it sets up for you. Let’s first see it in action:

testing_otp/rolling_average/thin_genserver_test_with_start_supervised.exs

```
defmodule RollingAverageServerTest do
  use ExUnit.Case

  describe "initialization" do
    test "accepts a measurement count on start" do
      assert {:ok, _pid} =
        start_supervised(
          {RollingAverageServer, max_measurements: 3}
        )
    end
  end
end
```