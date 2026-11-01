```
RollingAverageServer.add_element(pid, 7)

assert RollingAverageServer.average(pid) == 6.5
end
end
```

Although not as commonly used, ExUnit also provides a helper to shut down processes started with start_supervised/1 (or start_supervised!/1) before the test finishes executing. Such helper is ExUnit.Callbacks.stop_supervised/1 (with the mirroring stop_supervised!/1). The next test we’ll show is contrived and not the most useful, but it’ll help us show stop_supervised/1 in action, so bear with us.

```
testing_otp/rolling_average/thin_genserver_test_with_start_supervised.exs

describe "error cases" do
  test "asking for the average if the server has stopped crashes" do
    pid = start_supervised!({RollingAverageServer, max_measurements: 2})

    RollingAverageServer.add_element(pid, 5)
    RollingAverageServer.add_element(pid, 6)

    stop_supervised!(RollingAverageServer)

    assert {:noproc, _} = catch_exit(RollingAverageServer.average(pid))
  end
end
```

Ignore the call to catch_exit/1. We address that in the sidebar on catch_exit and catch_throw. As you can see, in this test we needed to shut down our GenServer before the end of the test. stop_supervised!/1 to the rescue! stop_supervised/1 and stop_supervised!/1 still shut down the started processes gracefully, so we don’t really have to worry about race conditions. One peculiarity to note is that these helpers take the child ID of the process to shut down and not its PID. This is in line with the APIs in the Supervisor module to shut down children, which tend to use the children’s IDs rather than their PIDs. In our case, the defaults in use