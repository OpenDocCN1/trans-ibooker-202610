It executes the test of a multi-module project (e.g., the examples repository) in parallel using four threads.

2 It executes the test of a multi-module project using the same number of threads as the CPU cores (for example, four threads in a Quad-Core system).

In addition, the plugin used to execute unit tests in Maven (called *Surefire*) provides two ways to run tests in parallel. The first one is multithreading inside a single JVM process. To enable this mode, we need to specify different configuration parameters, such as:

+   - `parallel`: To configure the level of granularity for parallelism. The possible values for this parameter are `methods` (to execute test methods in separate threads), `classes` (for test classes), `suites` (for test suites), `suitesAndClasses` (for test suites and classes), `suitesAndMethods` (for test suites and methods), and `all` (to execute every test in separate threads).
+   - `threadCount`: To define the maximum number of threads for parallelism.
+   - `useUnlimitedThreads`: To allow an unlimited number of threads.

There are two ways to specify these configuration parameters. First, we can configure them directly on the Maven configuration file (i.e., the `pom.xml` file). Example 8-30 demonstrates how. In addition, we can specify these parameters as system properties when using the command line, for instance, as follows:

```
mvn test -Dparallel=classesAndMethods -DthreadCount=4
```

# Example 8-30. Maven Surefire configuration sample for parallel exeaction

```
<build>
    <plugins>
```