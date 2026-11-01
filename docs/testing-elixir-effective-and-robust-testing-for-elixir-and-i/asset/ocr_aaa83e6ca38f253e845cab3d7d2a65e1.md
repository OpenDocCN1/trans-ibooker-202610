```
RollingAverage.add_element(pid, 5)
RollingAverage.add_element(pid, 6)
assert RollingAverage.average(pid) == 5.5

RollingAverage.add_element(pid, 7)
assert RollingAverage.average(pid) == 6.5
end
end
```

Having robust tests requires a little bit of dynamic setup and, as mentioned, we’re using a call that’s normally better to avoid (:sys.get_state/1), to allow the branching behavior to be tested without trying to interpret the results of the average/1 function. Violating the black box model is a test smell, but without it our tests would have to be numerous, because they’d have to set up various different states and interpret the values returned from average/1. That approach doesn’t just create a lot of tests to maintain; it also requires a decent-sized mental load for anyone trying to understand what the tests are doing.

This example is basic, but the more complex the internal logic, the harder it is to understand the tests. It becomes very tempting to start reaching into the GenServer for its state more and more. In the next section, we’ll take a look at how we can rethink our code so that it becomes more robust and at the same time easier to test.

# Isolating a Functional Core

An easier solution to this problem would be to move the logic out of the GenServer’s module and into a new, purely functional module. This allows you to take advantage of the ease of testing purely functional code while leaving the GenServer tests focused on what GenServer does best: maintaining state and presenting an API.

First, let’s look at the tests and code for the purely functional module, which we’ll call the RollingAverageMeasurements because it’ll now handle