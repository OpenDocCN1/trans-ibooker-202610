# Escaped Defect Rate Per Release

![](bbox=[0.12, 0.16, 0.88, 0.38])

Figure 9.6 Comparative escape rates.

In order to compare improvements from release to release the rates must be viewed as a snapshot in time boxed period of time. The time box can vary, depending on release cadence and customer adoption rates; however, it is generally best to limit it to one year from release for accurate comparison. The number of reported escapes can be plotted monthly per release for a year after each release. An escaped defect rate comparison chart could look like Figure 9.6.

A software development group can measure its progress toward improving software quality by tracking the release to release escape rate. In this particular case, it is apparent from this chart that the escaped defect rate is improving from release to release.

# 9.8 Code Coverage

Unit tests should strive to cover as much of the product code as possible. In a perfect world, unit test code would test 100% of the product code. However, not all code is testable by unit tests. Most of the unit test frameworks can generate a report on the LOC covered by the unit tests when they are executed in the framework. The coverage metric should be used as a yardstick to determine how adequate the unit tests are. Setting a coverage target is a bit controversial. Achieving 100% coverage is not feasible. As a rule of thumb, getting 90% or higher code coverage from unit tests is excellent. Less than 80% code coverage is probably inadequate. There are diminishing returns derived from trying to achieve 100% code coverage.

One thing to consider is that unit tests alone are not sufficient to guarantee that the product code is free of defects. Since software is stateful, simply executing a line of code once is not sufficient to test that code in all states. So, even if it were possible to get 100% code coverage with unit tests, the code under test is probably not completely free of defects. However, measuring code coverage is strongly recommended because code coverage metrics are one of the best ways to determine how comprehensively the code