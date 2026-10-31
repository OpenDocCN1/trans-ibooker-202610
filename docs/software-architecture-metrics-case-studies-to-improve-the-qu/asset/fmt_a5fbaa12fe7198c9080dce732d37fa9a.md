
* Limit circular dependency on the level of source files/classes. Any cycle group with more than five elements has a good chance to turn into code cancer and grow further until it becomes very difficult to untangle the mess. If you can, avoid even small cycle groups.
* Avoid code duplications (programming by copy and paste). I have not mentioned this before because this chapter is focused on metrics, but copy-and-paste programming is a classical [code smell](https://oreil.ly/xUtfn). Sometimes, duplicating code can be used to break cycles that are otherwise hard to break, but I would consider that a rare edge case.
* Limit the size of source files to 800 `LoC` (as a soft threshold).
* Limit maximum indentation to 4 and `Modified Cyclomatic Complexity` to 15 (as soft thresholds).

All of these rules can be implemented using what you’ve learned here. The best way to implement them is a tool-based approach where you automatically verify the rules in your CI build. It is also a good idea to implement those rules across the organization, so that all developers become familiar with them. Doing that would practically guarantee better maintainability, better code quality, and therefore much improved developer productivity. I have worked with several clients implementing such a rule-based approach and can confirm that it produces pretty impressive improvements over time.

# Conclusion

You’ve learned some useful metrics in this chapter, as well as the concept of a metrics-based feedback loop. And you now know that structural technical debt (or architectural debt) can really harm the productivity of development teams.

By now, it should be clear that metrics are a powerful tool that can help you discover harmful trends early enough to guarantee that your software project never ends up as a big ball of mud. Of course, you will need to use tools for that, some of which are even available for free. If you are not ready to use metrics on a wider scale yet, I highly recommend focusing on avoiding or at least limiting cyclic dependencies in your codebase. That by itself will stop the worst side effects of structural erosion and make it easier to adopt stricter rules further down the road.

^([1](#ch09.html#ch01fn73-marker)) Unfortunately, as of early 2022, Sonargraph-Explorer only supported Java, C#, and Python. For other languages, you might be forced to write a tool yourself or rely on a commercial solution.

^([2](#ch09.html#ch01fn74-marker)) First described by Carliss Baldwin, John Rusnak, and Alan MacCormack in “Exploring the Structure of Complex Software Designs: An Empirical Study of Open Source and Proprietary Code,” *Management Science* 52, no. 7 (2006): 1015–1030.

^([3](#ch09.html#ch01fn75-marker)) Ran Mo et al., [“Decoupling Level: A New Metric for Architectural Maintenance Complexity,”](https://oreil.ly/3zsqS) ICSE ’16, May 14–22, 2016, Austin, TX.

^([4](#ch09.html#ch01fn77-marker)) “PageRank,” *Wikipedia*, last updated April 9, 2022, *https://oreil.ly/vrdFb*.

^([5](#ch09.html#ch01fn78-marker)) Neal Ford, Rebecca Parsons, and Patrick Kua, “Chapter 2: Fitness Functions,” in *Building Evolutionary Architectures*.

# Chapter 10. Measure the Unknown with the Goal-Question-Metric Approach

Michael Keeling

In software, as in life, the most important things are often the most challenging to measure. How much technical debt is in the system, and where should you invest? How well does the architecture, as implemented, satisfy the most important quality attributes? How is the team’s design maturity progressing? For big questions like these, it’s easy to venture a guess based on gut feelings, which can be biased and unreliable. It’s far better to use data. When I need to define metrics for gnarly, difficult-to-answer problems like these, I turn to the goal-question-metric (GQM) approach.

The GQM approach is an analysis technique [proposed by Victor Basili and David Weiss](https://oreil.ly/SMvwj) to help teams figure out how to measure and evaluate tough problems in software development.^([1](#ch10.html#ch01fn79)) The technique is easy to learn and straightforward to apply. It can be used alone or as part of a collaborative workshop. Discovering the right metrics to evaluate a difficult problem still requires creativity and analytical thinking, but GQM provides just enough structure to nudge teams toward better outcomes.

In this chapter, you will learn how to use the GQM approach and facilitate a collaborative workshop with GQM at its heart. You’ll also see a case study about how a software development team used GQM to evaluate and improve gaps in their architecture. By the end of this chapter, you will know everything you need to apply GQM with your team.

## The Goal-Question-Metric Approach

The core assumption behind GQM is simple: to measure something well, you must understand why you’re measuring it. Understanding the *why*—the goals you want to accomplish and the questions you’ll need to answer to evaluate progress—gives you the power to identify and select the best metrics for the job. Teams that understand the *why* behind the metrics are more likely to use and trust those metrics to guide future decisions. GQM helps us transform goals from mushy statements about our desires into quantifiable and verifiable models.

The GQM model is hierarchical. If you imagine it as a tree, the goal is at the root. From the goal, the tree branches out to questions, then branches again to the metrics used to answer those questions. The leaves are the data used to compute the metrics. In this way, GQM creates traceability: you can trace a path from any individual leaf (the data being collected) back to the root (the purpose for collecting the data in the first place).

### Create a GQM Tree

In GQM, the *goal* is a simple statement that describes something you want to understand and measure. An ideal goal statement describes the purpose, the object to be measured, the issue or topic of interest, and the point of view from which you are considering the goal.

Here are a few examples of goals that focus on software architecture concerns:

* Improve system availability from the users’ point of view.
* Decrease the development time for new microservices from the product managers’ viewpoint.
* Reduce technical debt in the architecture from the perspective of software developers.
* Reduce the number of bugs being released into production.
* Detect more problems in production before our users do.
* Improve the machine learning model’s accuracy from the users’ viewpoint.
* Make better design decisions in the architecture from the development team’s perspective.

Providing the goal up front is essential to the process. The goal establishes the conceptual direction by focusing attention on a particular set of measures. The goal can focus on any object of interest, including elements of the architecture, software development processes, technical experiments, design artifacts, or even teams and organizations. Defining the object to be assessed aligns what is to be measured with why you need to measure it.

The goal is often revised throughout the analysis, as your understanding of what you’re measuring improves. Even an imperfect goal statement that captures only the essence of what needs to be measured is useful.

Once you define your goal, you can ask *questions* to explore and characterize it. Questions should be operationally focused and help you evaluate your progress against the goal: are you getting closer or farther away?

Great questions can illuminate problems and potential next steps for fixing those problems. Asking good questions requires a curious mind. You must be willing to temporarily let go of what is practical and ask the questions that need to be asked, even if you are unsure how to answer them now.

Here are some example questions that might help evaluate the goal of improving system availability from the users’ point of view:
