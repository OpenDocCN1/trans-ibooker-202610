number is dynamic). Then, we fill the state of the GenServer under test with this list and peek at the GenServer’s state to assert on the expected state. After that, we add a 5 to the GenServer, which causes the list of measurements to “overflow,” deleting the first added element (a 4). Finally, we once again peek at the GenServer state and verify that the oldest measurements have indeed been dropped and the new measurement added to the state.

When using dynamic values, it’s important to make sure they show up in the feedback on a failed test. In this case, you can see that max_measurements is pinned and will be part of the failure feedback. This makes it easier to reproduce the same conditions when debugging.

The add_element/2 test is doing a lot, but the biggest issue is that it can’t test anything without using a system call to peek into the GenServer and grab its state, seen at line 12. Our test has to know that the GenServer is storing the measurements as a list. This violates the concept of a black box around your code: the test code has to know the internal details of the code in order to pass. However, we want our tests to still pass if we change the internal details of our code but leave the public API unchanged. That would not happen here if we, for example, moved from a list to an Erlang queue (see :queue[21]) for the internal state representation. We warned you the test would be a bit nasty.

Our third test is a little simpler. In combination with the test before it, we have confidence that our “rolling” behavior works, so here we’re focusing just on the cast callback returning the average. Again, though, setting up state in our GenServer is a large part of the test.

```
testing_otp/rolling_average/overloaded_genserver_test.exs

describe "average/1" do
  test "it returns the average for the elements" do
    max_measurements = 2
    {:ok, pid} = RollingAverage.start_link(max_measurements)
```