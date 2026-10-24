# Understand Throughput, Batching, and Response Time

The second principle is to understand and select the appropriate test metric for the application. Performance can be measured as throughput (RPS), elapsed time (batch time), or response time, and these three metrics are interrelated. Understanding those relationships allows you to focus on the correct metric, depending on the goal of the application.

# Elapsed Time (Batch) Measurements

The simplest way to measure performance is to see how long it takes to accomplish a certain task. We might, for example, want to retrieve the history of 10,000 stocks for a 25-year period and calculate the standard deviation of those prices, produce a report of the payroll benefits for the 50,000 employees of a corporation, or execute a loop 1,000,000 times.

In statically compiled languages, this testing is straightforward: the application is written, and the time of its execution is measured. The Java world adds a wrinkle to this: just-in-time compilation. That process is described in Chapter 4; essentially it means that it takes anywhere from a few seconds to a few minutes (or longer) for the code to be fully optimized and operate at peak performance. For that (and other) reasons, performance studies of Java are concerned about warm-up periods: performance is most often measured after the code in question has been executed long enough for it to have been compiled and optimized.

# Other Factors for a Warm Application

Warming up an application is most often discussed in terms of waiting for the compiler to optimize the code in question, but other factors can affect the performance of code based on how long it has run.

The Java (or Jakarta) Persistence API (JPA), for example, will typically cache data it has read from the database (see Chapter 11); the second time that data is used, the operation will often be faster because the data can be obtained from the cache rather than requiring a trip to the database. Similarly, when an application reads a file, the operating system typically pages that file into memory. A test that subsequently reads the same file (e.g., in a loop to measure performance) will run faster the second time, since the data already resides in the computer’s main memory and needn’t be read from disk.

In general, there can be many places—not all of them obvious—where data is cached and where a warm-up period matters.