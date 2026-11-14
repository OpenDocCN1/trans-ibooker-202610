
| Name | Type | Concept | Component |
| --- | --- | --- | --- |
| *Async Daemon* |   |   |   |

## Examining the structure

After a thorough examination of the MR, it is time to dig into the files. You can use what you learned in chapter 2 to understand the structure of the new version of the code.

### Exercise 11.2: List all components

**Object components**

| Component name | Lines of Code | Size (small/medium/large) |
| --- | --- | --- |
| `AsyncDaemonHealthCheckExtensions` | 97 | small |

**Functional components**

| Method/function name | Lines of Code | Size (small/medium/large) |
| --- | --- | --- |
|   |   |   |

**Singular components**

| Variable name |   |   |
| --- | --- | --- |
|   |   |   |

## Understanding the domain concepts

Now that you have a clear understanding of the structure of the MR and the files, it is time to go back to the domain concepts.

Let's go back to the conversation tab of the MR, where you found the domain concept called *async daemon* and *health check*. From the text you can deduct that *async daemon* was an existing concept, where the functionality of a *health check* was implemented for. You wrote these concepts down but did nothing with them. Let's now use the documentation available to find the meaning of those words and record them in Exercise 11.3. This includes the new documentation that was added in this MR. You should also read the comments, if there are any, to see what other domain concepts are there.

### Exercise 11.3: Add all the domain concepts to the list and search for their meaning in the documentation.

| Name | Meaning |
| --- | --- |
| *Async Daemon* | *Asynchronous projection processing engine* |
| *Health check* |   |

Once you understand the meaning of old and new domain language, you can look at the tests to see how these concepts function. First examine the names of the tests, and give your own formulation as to what you think the test is doing. Take `` `should_add_settings_to_services` `` as a name. You can't easily deduct what exactly this is testing, even though you know the meaning of most of the domain concepts. Is it: Using the health check for the async daemon extension should add settings to services? Which services should have these settings now?

You also have to analyze if the test does what the name suggests. If you look at the test implementation of `` `should_be_healthy_with_one_projections_lagging` `` you see that the assert statement does not match the name of the test. The health check should return an unhealthy status according to the code: `` `result.Status.ShouldBe(HealthStatus.Unhealthy)` ``; This is something to add as a comment in the MR as shown in [Figure 5-6](#ch05.html#fig_6_5_add_a_comment_to_the_line_that_is_incorrect_acco).

![](assets/reading_for_code_reviews_309317_06.png)

Let's look at another domain concept. `` `HealthCheckResult` `` is a domain concept from the library that is being used in the code. I prefer to protect my code from leakages like this by introducing my own domain concept for this and translating from theirs to mine. It is also a very generic name, I could make it something more specific like `` `AsyncDaemonHealthStatus` ``, which tells me a lot more about the return value of this method than the original name.

I am also noticing an overuse here of `Unhealthy`. Three things can happen: the Async daemon is healthy, unhealthy or some kind of error occurs whilst trying to determine. In that last case, I have no idea what the health status is of the Async Daemon, yet they state that it is unhealthy. I would try to find a better way to do that.

### Exercise 11.4: Investigate domain concept `AsyncDaemonHealthCheckSettings`.

Perform an analysis of Async… similar to the one I did for the `HealthCheckResult`. Think about the following questions:

*   Is the code more readable because this domain concept is present?
*   Is this hiding another domain concept?
*   Is there a better name available for this?

## Converting improvements to tasks

If I want to go for non-blocking code reviews, as I mentioned in section 11.1, I can't just leave my comments and wait until the author of the code agrees with me or not. Because it is non-blocking, it also does not have to be the author that implements these either. Depending on how long it would take to implement my improvements I will either create well defined tasks to pick up later, or implement them immediately. Once I have done that, I am done with my review!

# Summary

When performing a code review, you need to use all four dimensions of code reading. The main purpose of a code review is to find improvements and share knowledge in the team. At the end of the code review, you will have a list of improvements. Depending on how long an improvement takes to implement, you can either implement them after merging back the branch or generate tasks out of them to pick up at a later time.

# About the Authors

**Felienne Hermans** is associate professor at Leiden Institute of Advanced Computer Science at Leiden University, where she heads the PERL research group, focused on programming education. She also works at the Vrije Universiteit Amsterdam, where she teaches prospective computer science teachers.

Felienne is the creator of the Hedy programming language, and was one of the founders of the Joy of Coding conference. Since 2016, she has been a host at SE radio, one of the most popular software engineering podcasts on the web. Felienne is the author of *The Programmer's Brain* (Manning 2021) which helps programmers understand how their brains work and how to use it more effectively. In 2021, Felienne was awarded the Dutch Prize for ICT research.

Felienne is a member of the board of I&I, the Dutch association of high-school computer science teachers, and of TC39, the committee that designs JavaScript.

**Gien Verschatse** is an experienced consultant and software engineer that specialises in domain modelling and software architecture. She's fluent in both object-oriented and functional programming, mostly in .NET. As a Domain-Driven Design practitioner, she always looks to bridge the gaps between experts, users, and engineers. As a side interest, she's researching the science of decision-making strategies, to help teams improve how they make technical and organisational decisions. She shares her knowledge by speaking and teaching at international conferences.

And when she is not doing all that, you'll find her on the sofa, reading a book and sipping coffee.
