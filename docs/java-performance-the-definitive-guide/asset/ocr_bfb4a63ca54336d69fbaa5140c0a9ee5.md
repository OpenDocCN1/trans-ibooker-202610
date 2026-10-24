# Threading and Synchronization Performance

From its first days, some of Java’s appeal has been because it is multithreaded. Even in the days before multicore and multi-CPU systems were the norm, the ability to easily write threaded programs in Java has been considered one of its hallmark features.

In performance terms, the appeal is obvious: if two CPUs are available, an application might be able to do twice as much work or the same amount of work twice as fast. This assumes that the task can be broken into discrete segments, since Java is not an autoparallelizing language that will figure out the algorithmic parts. Fortunately, computing today is often about discrete tasks: a server handling simultaneous requests from discrete clients, a batch job performing the same operation on a series of data, mathematical algorithms that break into constituent parts, and so on.

This chapter explores how to get the maximum performance out of Java threading and synchronization facilities.

# Threading and Hardware

Recall the discussion from Chapter 1 about multicore systems and hyper-threaded systems. Threading at the software level allows us to take advantage of a machine’s multiple cores and hyper-threads.

Doubling the cores on a machine allows us to double the performance of our correctly written application, though as we discussed in Chapter 1, adding hyper-threading to a CPU does not double its performance.

Almost all examples in this chapter are run on a machine with four single-threaded CPUs—the exception being the first example that shows the difference between hyper-threaded and non-hyper-threaded CPUs. After that, we will look at scaling