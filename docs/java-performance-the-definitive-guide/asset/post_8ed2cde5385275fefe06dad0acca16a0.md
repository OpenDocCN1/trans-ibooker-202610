
Table 2-3. Increased sample size of hypothetical times

|  | Baseline | Specimen |
|---|---|---|
| First iteration | 1.0 second | 0.5 second |
| Second iteration | 0.8 second | 1.25 second |
| Third iteration | 1.2 seconds | 0.5 second |
| Fourth iteration | 1.1 seconds | 0.8 second |
| Fifth iteration | 0.7 second | 0.7 second |
| Sixth iteration | 1.2 seconds | 0.75 second |
| Average | 1 second | 0.75 second |

With the additional data, the p-value drops from 0.43 to 0.11: the probability that the results are different has risen from 57% to 89%. The averages haven’t changed; we just have more confidence that the difference is not due to random variation.

Running additional tests until a level of statistical significance is achieved isn’t always practical. It isn’t, strictly speaking, necessary either. The choice of the α-value that determines statistical significance is arbitrary, even if the usual choice is common. A p-value of 0.11 is not statistically significant within a 90% confidence level, but it is statistically significant within an 89% confidence level.

Regression testing is important, but it’s not a black-and-white science. You cannot look at a series of numbers (or their averages) and make a judgment that compares them without doing some statistical analysis to understand what the numbers mean. Yet even that analysis cannot yield a completely definitive answer, because of the laws of probabilities. The job of a performance engineer is to look at the data, understand the probabilities, and determine where to spend time based on all the available data.

![](img/1711f142ae3cfd740ef106afbbba2046_48_0.png)

## Quick Summary

- Correctly determining whether results from two tests are different requires a level of statistical analysis to make sure that perceived differences are not the result of random chance.
- The rigorous way to accomplish that is to use Student’s t-test to compare the results.
- Data from the t-test tells us the probability that a regression exists, but it doesn’t tell us which regressions should be ignored and which must be pursued. Finding that balance is part of the art of performance engineering.

## Test Early, Test Often

Fourth and, finally, performance geeks (including me) like to recommend that performance testing be an integral part of the development cycle. In an ideal world, performance tests would be run as part of the process when code is checked into the central repository; code that introduces performance regressions would be blocked from being checked in.

Some inherent tension exists between that recommendation and other recommendations in this chapter—and between that recommendation and the real world. A good performance test will encompass a lot of code—at least a medium-sized mesobenchmark. It will need to be repeated multiple times to establish confidence that any difference found between the old code and the new code is a real difference and not just random variation. On a large project, this can take a few days or a week, making it unrealistic to run performance tests on code before checking it into a repository.

The typical development cycle does not make things any easier. A project schedule often establishes a feature-freeze date: all feature changes to code must be checked into the repository at an early point in the release cycle, and the remainder of the cycle is devoted to shaking out any bugs (including performance issues) in the new release. This causes two problems for early testing:

- Developers are under time constraints to get code checked in to meet the schedule; they will balk at having to spend time fixing a performance issue when the schedule has time for that after all the initial code is checked in. The developer who checks in code causing a 1% regression early in the cycle will face pressure to fix that issue; the developer who waits until the evening of the feature freeze can check in code that causes a 20% regression and deal with it later.
- Performance characteristics of code will change as the code changes. This is the same principle that argued for testing the full application (in addition to any module-level tests that may occur): heap usage will change, code compilation will change, and so on.

Despite these challenges, frequent performance testing during development is important, even if the issues cannot be immediately addressed. A developer who introduces code causing a 5% regression may have a plan to address that regression as development proceeds: maybe the code depends on an as-yet-to-be integrated feature, and when that feature is available, a small tweak will allow the regression to go away. That's a reasonable position, even though it means that performance tests will have to live with that 5% regression for a few weeks (and the unfortunate but unavoidable issue that said regression is masking other regressions).

On the other hand, if the new code causes a regression that can be fixed with only architectural changes, it is better to catch the regression and address it early, before the rest of the code starts to depend on the new implementation. It's a balancing act, requiring analytic and often political skills.

Early, frequent testing is most useful if the following guidelines are followed:

## Automate everything

All performance testing should be scripted (or programmed, though scripting is usually easier). Scripts must be able to install the new code, configure it into the full environment (creating database connections, setting up user accounts, and so on), and run the set of tests. But it doesn't stop there: the scripts must be able to run the test multiple times, perform t-test analysis on the results, and produce a report showing the confidence level that the results are the same, and the measured difference if they are not the same.

The automation must make sure that the machine is in a known state before tests are run: it must check that no unexpected processes are running, that the OS configuration is correct, and so on. A performance test is repeatable only if the environment is the same from run to run; the automation must take care of that.

## Measure everything

The automation must gather every conceivable piece of data that will be useful for later analysis. This includes system information sampled throughout the run: CPU usage, disk usage, network usage, memory usage, and so on. It includes logs from the application—both those the application produces, and the logs from the garbage collector. Ideally, it can include Java Flight Recorder (JFR) recordings (see Chapter 3) or other low-impact profiling information, periodic thread stacks, and other heap analysis data like histograms or full heap dumps (though the full heap dumps, in particular, take a lot of space and cannot necessarily be kept long-term).

The monitoring information must also include data from other parts of the system, if applicable: for example, if the program uses a database, include the system statistics from the database machine as well as any diagnostic output from the database (including performance reports like Oracle's Automatic Workload Repository, or AWR, reports).

This data will guide the analysis of any regressions that are uncovered. If the CPU usage has increased, it's time to consult the profile information to see what is taking more time. If the time spent in GC has increased, it's time to consult the heap profiles to see what is consuming more memory. If CPU time and GC time have decreased, contention somewhere has likely slowed performance: stack data can point to particular synchronization bottlenecks (see Chapter 9), JFR recordings can be used to find application latencies, or database logs can point to something that has increased database contention.

When figuring out the source of a regression, it is time to play detective, and the more data that is available, the more clues there are to follow. As discussed in Chapter 1, it isn’t necessarily the case that the JVM is the regression. Measure everything, everywhere, to make sure the correct analysis can be done.

## Run on the target system

A test that is run on a single-core laptop will behave differently than a test run on a machine with 72 cores. That should be clear in terms of threading effects: the larger machine is going to run more threads at the same time, reducing contention among application threads for access to the CPU. At the same time, the large system will show synchronization bottlenecks that would be unnoticed on the small laptop.

Other performance differences are just as important, even if they are not as immediately obvious. Many important tuning flags calculate their defaults based on the underlying hardware the JVM is running on. Code is compiled differently from platform to platform. Caches—software and, more importantly, hardware—behave differently on different systems and under different loads. And so on…

Hence, the performance of a particular production environment can never be fully known without testing the expected load on the expected hardware. Approximations and extrapolations can be made from running smaller tests on smaller hardware, and in the real world, duplicating a production environment for testing can be quite difficult or expensive. But extrapolations are simply predictions, and even in the best case, predictions can be wrong. A large-scale system is more than the sum of its parts, and there can be no substitute for performing adequate load testing on the target platform.

![](img/1711f142ae3cfd740ef106afbbba2046_51_0.png)

## Quick Summary

- Frequent performance testing is important, but it doesn’t occur in a vacuum; there are trade-offs to consider regarding the normal development cycle.
- An automated testing system that collects all possible statistics from all machines and programs will provide the necessary clues to any performance regressions.

## Benchmark Examples

Some examples in this book use jmh to provide a microbenchmark. In this section, we’ll look in depth at how one such microbenchmark is developed as an example of how to write your own jmh benchmark. But many examples in this book are based on variants of a mesobenchmark—a test that is complex enough to exercise various JVM
