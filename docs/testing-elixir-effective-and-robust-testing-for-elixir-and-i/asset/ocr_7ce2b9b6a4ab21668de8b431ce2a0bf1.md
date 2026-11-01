As you can see, the changes we made are minimal. Now, if we pass mode: :manual when starting the GenServer, the GenServer won't initiate the ticking loop. That means we need a way to manually tick. This is going to sound horrible, but we think the best way is to send the GenServer a :tick message to simulate exactly what mode: :periodic would do. It sounds bad because it breaks the black box model by relying on an internal implementation detail of our GenServer. However, we believe that it's a simple and practical solution that achieves the purposes of testing: if we change the internal implementation, the test will break, which is not ideal but at least will signal that there's something to fix and won't slip through the cracks instead. We could come up with more elegant ways of hiding this implementation detail, such as exposing a tick/0 function in the GenServer, but we believe that keeping the changes to the application code to a minimum yields more benefits than hiding this detail.

Now our test can start the GenServer in :manual mode and use Mox.expect/4 to assert that send_sms/2 is called exactly once during the test:

```
testing_otp/periodic_actions/weather_checker_with_mode_test.exs

defmodule SoggyWaffle.WeatherCheckerTest do
  use ExUnit.Case, async: true

  # same setup as before,

  test "when the process \"ticks\", the Twilio interface is called" do
    interval_in_ms = 5
    phone_number = "+1 111 11 1111"
    test_pid = self()
    ref = make_ref()

    start_options = [
      # The :mode option is set explicitly for testing.
      mode: :manual,
      interval: interval_in_ms,
```