# 10

# Additional Information

In this chapter, we will cover the following topics:

+   - Processing results for Runnable objects in the Executor framework
+   - Processing uncontrolled exceptions in a ForkJoinPool class
+   - Using a blocking thread-safe queue to communicate with producers and consumers
+   - Monitoring a Thread class
+   - Monitoring a Semaphore class
+   - Generating concurrent random numbers

# Introduction

This chapter include recipes about the Executor framework and the fork/join framework, concurrent data structures, monitoring concurrent objects, and generating concurrent random numbers.

# Processing results for Runnable objects in the Executor framework

The Executor framework allows the execution of concurrent tasks that returns a result using the Callable and Future interfaces. The traditional concurrent programming in Java is based on Runnable objects, but this kind of object doesn't return a result.