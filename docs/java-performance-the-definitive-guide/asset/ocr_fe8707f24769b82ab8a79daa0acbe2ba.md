# Understand Variability

The third principle is to understand how test results vary over time. Programs that process exactly the same set of data will produce a different answer each time they are run. Background processes on the machine will affect the application, the network will be more or less congested when the program is run, and so on. Good benchmarks also never process exactly the same set of data each time they are run; random behavior will be built into the test to mimic the real world. This creates a problem: when comparing the result from one run to the result from another run, is the difference due to a regression or to the random variation of the test?

This problem can be solved by running the test multiple times and averaging those results. Then when a change is made to the code being tested, the test can be rerun multiple times, the results averaged, and the two averages compared. It sounds so easy.

Unfortunately, it isn’t as simple as that. Understanding when a difference is a real regression and when it is a random variation is difficult. In this key area, science leads the way, but art will come into play.

When averages in benchmark results are compared, it is impossible to know with absolute certainty whether the difference in the averages is real or due to random fluctuation. The best that can be done is to hypothesize that “the averages are the same” and then determine the probability that such a statement is true. If the statement is false with a high degree of probability, we are comfortable believing the difference in the averages (though we can never be 100% certain).

Testing code for changes like this is called regression testing. In a regression test, the original code is known as the baseline, and the new code is called the specimen. Take the case of a batch program in which the baseline and specimen are each run three times, yielding the times given in Table 2-2.

Table 2-2. Hypothetical times to execute two tests

| | Baseline | Specimen |
|---|---|---|
| First iteration | 1.0 second | 0.5 second |
| Second iteration | 0.8 second | 1.25 seconds |
| Third iteration | 1.2 seconds | 0.5 second |
| Average | 1 second | 0.75 second |

The average of the specimen indicates a 25% improvement in the code. How confident can we be that the test really reflects a 25% improvement? Things look good: two of the three specimen values are less than the baseline average, and the size of the improvement is large. Yet when the analysis described in this section is performed on those results, it turns out that the probability of the specimen and the baseline having