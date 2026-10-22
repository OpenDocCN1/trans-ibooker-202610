+   - memUtilPct = this.memUtilTask.getTelemetry()
+   - Log an info message that includes the values of cpuUtilPct and memUtilPct

+   - Within the constructor, add the following:

```
this.schedExecSvc =
Executors.newScheduledThreadPool(1);
this.sysCpuUtilTask = new SystemCpuUtilTask();
this.sysMemUtilTask = new SystemMemUtilTask();
this.taskRunner = () -> {
    this.handleTelemetry();
};
```

+   - Within the startManager() method, add the following:

```
if (! this.isStarted) {
    ScheduledFuture<?> futureTask =
    this.schedExecSvc.scheduleAtFixedRate(this.taskRunner,
    0L, this.pollSecs, TimeUnit.SECONDS);
    this.isStarted = true;
}
```

+   - Within the stopManager() method, add the following:
+   this.schedExecSvc.shutdown();

Estimate (Small = < 2 hrs ; Medium = 4 hrs ; Large = 8 hrs )

+   - Medium

# Tests

+   - In the GatewayDeviceAppTest test case, comment out the @Test annotation before the testRunGatewayApp() unit test, and uncomment the @Test annotation before the testRunTimedGatewayApp unit test. Run the latter test - it should yield output similar to the following: