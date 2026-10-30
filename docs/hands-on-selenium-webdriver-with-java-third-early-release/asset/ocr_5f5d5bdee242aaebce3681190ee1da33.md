+   - `junit.jupiter.execution.parallel.mode.classes`
+     - **default:** To run test classes in parallel. The possible values are `same_thread` for single threaded execution (by default) and `concurrent` for parallel execution.
+   - `junit.jupiter.execution.parallel.mode.default`
+     - To run test methods in parallel. The possible values are the same as before (for test classes).

There are two ways of specifying these parameters. First, in the configuration file `junit-platform.properties` (that should be available in the project classpath). **Example 8-33** shows a sample content of this file. Second, by using system properties and the command line. The following commands (Maven/Gradle) show how:

```
mvn test -Djunit.jupiter.execution.parallel.enabled=true
gradle test -Djunit.jupiter.execution.parallel.enabled=true
```

# *Example 8-33. Parallel test execution using JUnit 5*

```
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = same_thread
junit.jupiter.execution.parallel.mode.classes.default = same_thread
```

In addition, the Jupiter programming model provides the annotation `@Execution` to change the parallelization mode for test classes or methods. To that aim, this annotation can be used at class-level or method-level and accepts two values: `ExecutionMode.CONCURRENT` (for parallel execution) and `ExecutionMode.SAME_THREAD` (for single-threaded execution). **Example 8-34** shows the structure of a test class contained in the example repository. Supposing that parallel test is enabled (like in **Example 8-33**), this class will be executed in parallel together with other tests that allow parallelization.

# *Example 8-34. Parallel test execution using JUnit 5*

```
```java
@Execution(ExecutionMode.CONCURRENT)
class ParallelJupiterTest {

    // Test logic
```
```