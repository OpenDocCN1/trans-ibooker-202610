Although your GDA isn’t collecting sensor data, it clearly needs to gather and assess its own system performance (such as CPU and memory utilization, of course). This type of functionality is typically collected in the background at regular intervals, and as with Python, Java provides options for creating polling systems.

Fortunately, there’s no need to import any separate libraries, however, because this is built-in to the core Java SDK.

# NOTE

Java’s concurrency library is quite powerful, and allows you to use a basic Timer functionality as well as a ScheduledExecutorService (you can also create your own threaded polling system if you really want to, of course). We’ll use ScheduledExecutorService, as it provides a semi-guaranteed way to poll at regular intervals, handling most of the complexity for us. Modern Java virtual machines will handle the load distribution across the CPU architecture, meaning it will utilize multiple cores if at all possible.

# INTEGRATING SYSTEMCPUUTIL TASK AND SYSTEMMEMUTIL TASK WITH SYSTEMPERFORMANCEMANAGER

In this section, you’ll create two very similar tasks: *PIOT-GDA-02-006 - Connect SystemCpuUtilTask and SystemMemUtilTask to SystemPerformanceManager.*

This particular exercise is a bit more involved, even though it doesn’t take many lines of code to complete. This is because you’ll be using both concurrency and a *Runnable* implementation,