
A passing green build means our changes have safely been integrated. A red build means the last change possibly did not integrate. You need to stop all further check-ins that aren’t involved in fixing the builds to get it passing again. If you let more changes pile up, the time it takes to fix the build will increase drastically. I’ve worked with teams where the build has been broken for days, resulting in substantial efforts to eventually get a passing build.

# Branching Models

Few topics around build and deployment seem to cause as much of a controversy as that of using source code branching for feature development. Branching in source code allows for development to be done in isolation, without disrupting the work being done by others. On the surface of it, creating a source code branch for each feature being worked on - otherwise known as feature branching - seems like a useful concept.

The problem is that when you work on a feature branch, you aren’t regularly integrating your changes with everyone else. Fundamentally, you are *delaying* integration. And when you finally decide to integrate in your changes with everyone else, you’ll have a much more complex merge.

The alternative approach is to have everyone check in to the same “trunk” of source code. To keep changes from impacting other people, techniques like feature flags are used to “hide” incomplete work. This technique of everyone working off the same trunk is called *Trunk-Based Development*.

The discussion around this topic is nuanced, but my own take on this is that the benefits of frequent integration - and validation of that integration - are significant enough that Trunk-Based Development is my prefered style of development. Moreover, the work to implement feature flags is frequently beneficial in terms of progressive delivery, a concept we’ll explore in [Chapter 7](#ch07.xhtml#deployment_chapter).

## Be Careful About Branches

Integrate early, and integrate often. Avoid the use of long-lived branches for feature development, and consider Trunk-Based Development instead. If you really have to use branches, keep them short!

Quite aside from my own anecdotal experience, there is a growing body of research that shows the efficacy of reducing the number of branches and adopting Trunk-Based Development. The 2016 State Of DevOps report by DORA and Puppet^([1](#ch06.xhtml#idm46534870088008)) carries out a rigorous research into the delivery practices of organizations around the world and studies which practices are commonly used by high performing teams:

> We found that having branches or forks with very short lifetimes (less than a day) before being merged into trunk, and less than three active branches in total, are important aspects of continuous delivery, and all contribute to higher performance. So does merging code into trunk or master on a daily basis.
> 
> State Of Devops Report 2016

In subsequent years, the State of Devops report has continued to explore this topic in more depth, and has continued to find evidence for the efficacy of this approach.

A branch-heavy approach is still common in open source development, often through adopting the “GitFlow” development model. It’s worth noting that open source development is not the same as normal day-to-day development. Open source development is characterized by a large number of ad-hoc contributions from time-poor “untrusted” committers, whose changes require vetting by a smaller number of “trusted” contributors. Typical day-to-day closed source development is normally done by a tight-knit team who all have commit rights, even if they decide to adopt some form of code review process. So what might work for open source development may not work for your day job. Even then, the State Of Devops report for 2019^([2](#ch06.xhtml#idm46534870082328)), further exploring this topic, found some interesting insights into open source development and the impact of “long lived” branches:

> Our research findings extend to open source development in some areas:
> 
> *   Committing code sooner is better: In open source projects, many have observed that merging patches faster to prevent rebases helps developers move faster.
> 
> *   Working in small batches is better: Large “patch bombs” are harder and slower to merge into a project than smaller, more readable patchsets since maintainers need more time to review the changes.
> 
> Whether you are working on a closed-source code base or an open source project, short-lived branches; small, readable patches; and automatic testing of changes make everyone more productive.
> 
> State Of Devops Report 2019

# Build Pipelines and Continuous Delivery

Very early on in doing CI, my then-colleagues at ThoughtWorks and I realized the value in sometimes having multiple stages inside a build. Tests are a very common case where this comes into play. I may have a lot of fast, small-scoped tests, and a small number of large-scoped, slow tests. If we run all the tests together, and if we’re waiting for our long-scoped slow tests to finish, we may not be able to get fast feedback when our fast tests fail. And if the fast tests fail, there probably isn’t much sense in running the slower tests anyway! A solution to this problem is to have different stages in our build, creating what is known as a *build pipeline*. One stage for the faster tests, which if it passes then triggers a separate stage for the slower tests.

This build pipeline concept gives us a nice way of tracking the progress of our software as it clears each stage, helping give us insight into the quality of our software. We create deployable artifact, the thing that will ultimately be deployed into production, and use this artifact throughout the pipeline. In our context, this artifact will relate to a microservice we want to deploy. In [Figure 6-1](#ch06.xhtml#sample-pipeline) we see this happening - the same artifact is used in each stage of the pipeline, giving us more and more confidence that the software will work in production.

![](Images/sample-pipeline.png)

*Continuous Delivery (CD)* builds on this concept, and then some. As outlined in Jez Humble and Dave Farley’s book of the same name^([3](#ch06.xhtml#idm46534870068136)), CD is the approach whereby we get constant feedback on the production readiness of each and every check-in, and furthermore treat each and every check-in as a release candidate.

To fully embrace this concept, we need to model all the processes involved in getting our software from check-in to production, and know where any given version of the software is in terms of being cleared for release. In CD, we do this by modelling each and every stage our software has to go through, both manual and automated, an example of which we shared just a moment ago for our `Catalog` service in [Figure 6-1](#ch06.xhtml#sample-pipeline). Most CI tools nowadays provide some support for defining and visualizing the state of build pipelines like this

If the new `Catalog` service passes whatever checks are carried out at a stage in the pipeline, it can then move on to the next step. If it doesn’t pass a stage, our CI tool can let us know which stages that build has passed, and can get visibility about what failed. If we need to do something to fix it, we’d make a change and check it in, allowing the new version of our microservice to try and pass all the stages before being available for deployment. In [Figure 6-2](#ch06.xhtml#failing-pipeline), we see an example of this. `build-120` failed the fast test stage, `build-121` failed at the performance tests, but `build-122` made it all the way to production.

![](Images/failing-pipeline.png)

## Tooling
