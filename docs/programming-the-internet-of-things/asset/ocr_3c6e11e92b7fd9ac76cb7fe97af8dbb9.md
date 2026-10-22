+   - Create a class within the module named SystemPerformanceManager.
+   - Add the startManager() method, log an info message indicating manager was started.
+   - Add the stopManager() method, log an info message indicating manager was stopped.

# Estimate (Small = < 2 hrs ; Medium = 4 hrs ; Large = 8 hrs )

+   - Small

# Tests

+   - Run the SystemPerformanceManagerTest unit test. The log output should look similar to the following:

```
2020-07-06 21:03:03,654:INFO:Initializing
SystemPerformanceManager...
2020-07-06 21:03:03,655:INFO:Started
SystemPerformanceManager.
2020-07-06 21:03:03,656:INFO:Stopped
SystemPerformanceManager.
```

This component will actually live in a different folder than the application, so let’s create that now. In your IDE, navigate to ‘src -> main -> python -> programmingtheiot -> cda’, and create a new folder named ‘system’.

The requirements for SystemPerformanceManager are pretty straight forward, so see if you can implement these on your own. You can always review the solutions located in my python-solutions GitHub page if you need a hint (https://github.com/programmingtheiot/python-solutions)!