
As you can see from the examples, the categorization of real-world examples is not always a black-and-white decision for each category. But this is also not the goal for the categories. The categories or dimensions should be used as a catalog of important aspects of fitness functions and their metrics first. The second goal is that they assist software architects in identifying areas currently not covered, helping them decide where additional efforts may be needed.

# Developing Your Fitness Functions and Metrics

Let’s look at how I develop fitness functions and continue the ongoing and iterative efforts once an initial set is implemented. For me, the main starting point for all activities related to software architecture work are the system’s quality goals. This is also the starting activity I recommend when you work on fitness functions and metrics for your system.

If you haven’t aligned on the main quality goals with the system’s stakeholders, this is a good opportunity to start doing so. Start by collecting the relevant quality goals and create a shared vision of the quality goals that is agreed upon by all stakeholders of your system.

Ford, Parsons, and Kua argue that “Teams should identify fitness functions as part of their initial understanding of the overall architecture concerns that their design must support. They should also identify their system fitness function early to help determine the sort of change that they want to support.”^([13](#ch02.html#ch01fn38)) While in general I support this idea, it is hard to identify all fitness functions right from the beginning, just as it is hard to know all your requirements when you start. I recommend starting off small and easy and then learning while you implement the system. Use these learnings to improve, change, or add new fitness functions and metrics as needed. Start with relevant tests in the bottom layer of the pyramid.

The following process, which I would fully integrate into an iterative development process like Scrum, could help you define your first fitness functions and implement architectural tests:

1. Work with key stakeholders to identify the most important quality attributes, set architectural goals, and document them.
Using goals avoids the pitfall of creating fitness functions and automated tests that don’t add value to your system. I have often seen people automate tests not aimed at key quality goals that added little to no value, just because it was easy. Focusing on the main architectural goals provides a sense of purpose.

2. Formulate first drafts of fitness functions and their target metrics.
Think about the dimensions that are important to you. Put the draft versions in a list that is shared with the whole team (you could also use your backlog). Document the categories you already anticipate are relevant.
Why a shared list or backlog? Defining the right fitness functions and creating an automated test that produces the target metric involves some effort. With the backlog, you can collect your ideas until you are ready to implement them.
Documenting categories for each fitness function draft can also be useful later to select fitness functions in new areas, such as relevant quality goals of your system you haven’t covered so far, or to balance your testing portfolio by adding tests from a different layer.

3. Prioritize and select fitness functions that are important, helpful, and feasible to test for at the moment.
Consider areas and dimensions not covered by some tests, as well as the pyramid layers. Do you already have a balanced portfolio of architectural tests and metrics? If you are doing this process for the first time, start with something simple from the bottom of the pyramid.

4. Finalize any unfinished fitness function definitions among your selections.
Keep the full definition and its classification in the pyramid’s layer. During the next iteration of the process, this provides a valuable overview of the areas you’ve already covered.

5. Develop an automated test that can produce the metric.
Ideally, you want to verify these tests often. Depending on the type of fitness function and its definition, you should usually also confirm that the test directly verifies that target metric. I highly recommend automating it by default; only a few very specific metrics should be created and verified manually.

6. Visualize the results.
Use a dashboard or another form of visualization to share your results with the whole team and, when needed, with relevant stakeholders.

7. Iterate regularly as needed.
Perhaps some tests’ output is unreliable, a certain kind of metric is not useful anymore, or the maintenance efforts are too high. Decommission tests and fitness functions that don’t provide enough value or are just not needed anymore.
Change existing fitness functions and metrics as needed, too; for example, you can make existing metrics stricter to reflect improvements in the system, or less strict if they are not as relevant to your overall system goals.

# Conclusion

Testing your system arbitrarily can lead you to lose focus on what’s most important: overall quality in relevant areas such as performance, security, and modifiability. You should begin with a solid foundation of architectural tests that are relatively easy to build and which you can develop and extend as needed. Using fitness functions as a method to create metrics tailored to your system allows for customization, and following the process I’ve described here helps to reduce unneeded overhead. The fitness function test pyramid provides an additional layer to classify your tests and balance out your efforts. The metrics are the final objective: they are the unbiased measures that keep a team focused on the aligned goals of the software system.

^([1](#ch02.html#ch01fn25-marker)) Ford, Parsons, and Kua, *Building Evolutionary Architectures* (O’Reilly, 2017), [Chapter 1](https://oreil.ly/tWGOM).

^([2](#ch02.html#ch01fn26-marker)) Cyclomatic complexity is a widely used code metric that is usually derived using static code analysis tools. It was developed by Thomas J. McCabe in 1976. A high value indicates that the code is difficult to understand and may be difficult to change.

^([3](#ch02.html#ch01fn27-marker)) *Test coverage* is a term in software development to measure the degree of how much source code is covered by a certain set of tests. For instance, it is very common to measure the lines of code (“line coverage”) that have been touched (“tested”) while automated tests are executed. There are also other ways to measure (such as branch coverage), but I am sticking with line coverage for the simple explanation: if a program with 100 lines of code has test coverage of 80%, 80 lines of this code have been touched by the test execution.

^([4](#ch02.html#ch01fn28-marker)) Ford, Parsons, and Kua, *Building Evolutionary Architectures*.

^([5](#ch02.html#ch01fn29-marker)) Len Bass, Paul Clements, and Rick Kazman, *Software Architecture in Practice*, 3rd ed. (Westford, MA: Addison-Wesley, 2015), p. 64.

^([6](#ch02.html#ch01fn31-marker)) See, for example, Ashley Davis, *Bootstrapping Microservices with Docker, Kubernetes, and Terraform* (Manning, 2021); Lisa Crispin and Janet Gregory, *Agile Testing: A Practical Guide for Testers and Agile Team* (Addison-Wesley, 2008); and Martin Fowler, “TestPyramid,” [*MartinFowler.com*](http://MartinFowler.com), May 1, 2012, *https://oreil.ly/9o1DV*.

^([7](#ch02.html#ch01fn32-marker)) Fowler, “TestPyramid.”

^([8](#ch02.html#ch01fn33-marker)) George E. P. Box, “Science and Statistics,” *Journal of the American Statistical Association* 71, no. 356 (December 1976): 791–799.
