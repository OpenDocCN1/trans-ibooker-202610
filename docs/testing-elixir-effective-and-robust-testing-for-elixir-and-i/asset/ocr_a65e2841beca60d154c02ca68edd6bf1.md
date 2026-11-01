```
Process.sleep(100)
wait_for_passing(timeout - 100, fun)
end

defp wait_for_passing(_timeout, fun), do: fun.()
```

The implementation is straightforward and uses recursion. It decreases the given timeout until it “runs out.” In this implementation, we’re hard-coding Process.sleep(100) and timeout - 100, which means that the assertion is run every 100 milliseconds during the given interval, but we could change this value or turn it into an argument to the wait_for_passing function to make it customizable. wait_for_passing/2 isn’t the most efficient function since it repeatedly runs the assertion and “wastes” a few hundred milliseconds between runs, but it’s a good and effective tool that in the real world we’ve ended up using in more than a few places.

This section has turned out to be more about the code to test than the tests themselves, but we believe it serves the purpose of showing what it means to test cleaning up after crashes. In this instance, the test was small and simple enough. If things become more complicated, you’ll have the tools we learned about in the previous chapters to help you architect your tests using things like dependency doubles and message passing.

Let’s move on to the final enemy of easy testing in the OTP landscape: supervisors.

# Testing Supervision Trees

Supervisors are one of the strongest selling points of Erlang/Elixir and the OTP set of abstractions. They allow you to structure the life cycle of the processes inside your application in a resilient way, and they make isolating failures a breeze. However, supervisors are one of the toughest things to test that we’ve come across. The reason for this is that their main job is to allow your application to recover from complex