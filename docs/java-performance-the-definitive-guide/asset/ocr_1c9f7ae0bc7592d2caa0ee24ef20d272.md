Earlier in this chapter, Table 9-1 showed the effect on the performance of the parallel stock history calculations when the pool had various sizes. Table 9-9 compares that data to the forEach() construct using the common ForkJoinPool (with the parallelism system property set to the given value).

# Table 9-9. Time required to calculate 10,000 mock price histories

| Number of threads | ThreadPoolExecutor | ForkJoinPool |
| 1 | 40 ± 0.1 seconds | 20.2 ± 0.2 seconds |
| 2 | 20.1 ± 0.07 seconds | 15.1 ± 0.05 seconds |
| 4 | 10.1 ± 0.02 seconds | 11.7 ± 0.1 seconds |
| 8 | 10.2 ± 0.3 seconds | 10.5 ± 0.1 seconds |
| 16 | 10.3 ± 0.03 seconds | 10.3 ± 0.7 seconds |

By default, the common pool will have four threads (on our usual four-CPU machine), so the third line in the table is the common case. The results for a common pool size of one and two are exactly the sort of result that should give a performance engineer fits: they seem to be completely out of line because ForkJoinPool is performing far better than might be expected.

When a test is out of line like that, the most common reason is a testing error. In this case, however, it turns out that the forEach() method does something tricky: it uses both the thread executing the statement and the threads in the common pool to process the data coming from the stream. Even though the common pool in the first test is configured to have a single thread, two threads are used to calculate the result. Consequently, the time for a ThreadPoolExecutor with two threads and a ForkJoinPool with one thread is essentially the same.

If you need to tune the size of the common pool when using parallel stream constructs and other autoparallel features, consider decreasing the desired value by one.

![](bbox=[0.15, 0.65, 0.24, 0.75])

# Quick Summary

+   - The ForkJoinPool class should be used for recursive, divide-and-conquer algorithms. This class in not suited for cases that can be handled via simple partitioning.
+   - Make the effort to determine the best point at which the recursion of tasks in the algorithm should cease. Creating too many tasks can hurt performance, but too few tasks will also hurt performance if the tasks do not take the same amount of time.
+   - Features that use automatic parallelization will use a common instance of the ForkJoinPool class. You may need to adjust the default size of that common instance.