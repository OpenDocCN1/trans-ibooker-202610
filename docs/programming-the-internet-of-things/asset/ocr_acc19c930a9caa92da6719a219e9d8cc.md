the latter of which is just an interface definition for a method that can be invoked one or more times by a Java thread.

Let’s document this work using the following description:

# Description

+   - Connect SystemCpuUtilTask and SystemMemUtilTask into SystemPerformanceManager, and call each instance’s handleTelemetry() method from within a thread that starts when the manager is started, and stops when the manager is stopped. This work should be implemented within the SystemPerformanceManager class.

# Actions

+   - Add the following import statements:

```
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.ScheduledFuture;
import java.util.concurrent.TimeUnit;
```

+   - Add the following members to the class:

```
private ScheduledExecutorService schedExecSvc = null;
private SystemCpuUtilTask sysCpuUtilTask = null;
private SystemMemUtilTask sysMemUtilTask = null;
private Runnable taskRunner = null;
private boolean isStarted = false;
```

+   - Create a public method named handleTelemetry() that includes the following:
+     - cpuUtilPct = this.cpuUtilTask.getTelemetry()