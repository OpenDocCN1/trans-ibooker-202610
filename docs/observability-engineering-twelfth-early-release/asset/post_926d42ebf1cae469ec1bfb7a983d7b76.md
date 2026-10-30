
## Chapter 9. How Observability and Monitoring Come Together

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 9th chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

So far in this book, we’ve examined the differentiating capabilities of observable systems, the technological components necessary for observability, and how observability fits into the technical landscape. Observability is fundamentally distinct from monitoring, and both serve different purposes. In this chapter, we examine how they fit together and the considerations for determining how both may coexist within your organization.

Many organizations have years—if not decades—of accumulated metrics data and monitoring expertise set up around their production software systems. As covered in earlier chapters, traditional monitoring approaches are adequate for traditional systems. But when managing modern systems, does that mean you should throw all that away and start fresh with observability tools? Doing that would be both cavalier and brash. The truth for most organizations is that their approach to coexisting approaches should be dictated by their adopted responsibilities.

This chapter explores how observability and monitoring come together by examining the strengths of each, the domains to where they are best suited, and the ways they complement one another. Every organization is different, and a recipe for the coexistence of observability and monitoring cannot be universally prescribed. However, a useful guideline is that observability is best suited to understanding issues at the application level and that monitoring is best for understanding issues at the system level. By considering your workloads, you can figure out how the two come together best for you.

## Where Monitoring Fits

In Chapter 2, we focused on differentiating observability and monitoring. That chapter mostly focuses on the shortcomings of monitoring systems and how observability fills in those gaps. But monitoring systems still continue to provide valuable insights. Let’s start by examining where traditional monitoring systems continue to be the right tool for the job.

The traditional monitoring approach to understanding system state is a mature and well-developed process. Decades of iterative improvement have evolved monitoring tools beyond their humble beginnings with simple metrics and round-robin databases (RRDs), toward TSDBs and elaborate tagging systems. A wealth of sophisticated options also exist to provide this service—from open source software solutions, to start-ups, to publicly traded companies.

Monitoring practices are well-known and widely understood beyond the communities of specialists that form around specific tools. Across the software industry, monitoring best practices exist that anyone who has operated software in production can likely agree upon.

For example, a widely accepted core tenet of monitoring is that a human doesn’t need to sit around watching graphs all day; the system should proactively inform its users when something is wrong. In this way, monitoring systems are reactive. They react to known failure states by alerting humans that a problem is occurring.

Monitoring systems and metrics have evolved to optimize themselves for that job. They automatically report whether known failure conditions are occurring or about to occur. They are optimized for reporting on unknown conditions about known failure modes (in other words, they are designed to detect *known unknowns*).

The optimization of monitoring systems to find known unknowns means that it’s a best fit for understanding the state of your systems, which change much less frequently and in more predictable ways than your application code. By *systems*, we mean your infrastructure, or your runtime, or counters that help you see when you’re about to slam up against an operating constraint.

As seen in Chapter 1, metrics and monitoring were created to examine hardware-level performance. Over time, they’ve been adapted to encompass a wider range of infrastructure and system-level concerns. Most readers of this book, who work in technology companies, should recognize that the underlying systems are not what matters to your business. At the end of the day, what matters to your business is how the applications you wrote perform in the hands of your customers. The only reason your business is concerned about those underlying systems is that they could negatively impact application performance.

For example, you want to know if CPU is pegged on a virtual instance with a noisy neighbor because that tells you the latency you’re seeing isn’t an issue inside your code. Or if you see that physical memory is close to being exhausted across your entire fleet, that tells you an impending disaster probably originated from your code. Correlating system constraints with application performance matters, but system performance matters mostly as a warning signal or a way to rule out code-based issues.

Over time, metrics have also been adapted to creep into monitoring application-level concerns. But as you’ve seen throughout Part I, these aggregate measures are far too coarse because they cannot be decomposed down to show the performance of individual requests in your services. In the role of a warning signal, aggregate measures like metrics work well. But

metrics aren’t, and never have been, a good way to indicate how the code you wrote behaves in the hands of individual users.

## Where Observability Fits

In contrast to monitoring, observability has different tenets and use cases. As seen in Chapter 2, observability is more proactive. Its practices demand that engineers should always watch their code as it is deployed, and should spend time every day exploring their code in production, watching users use it, and looking around for outliers and curious trails to follow.

As covered in Chapter 8, the core analysis loop enables debugging from first principles and is tailored to discovering previously unknown failure modes (in other words, it is designed to detect *unknown unknowns*). The optimization of observability to find unknown unknowns means it’s a best fit for understanding the state of the code you write, which changes much more frequently than your systems (typically, every day) and in far less predictable ways.

Monitoring and observability tools have different best practices and different implementations, and they serve different purposes.

## System Versus Software Considerations

In more traditional settings, the distinction between systems and software was clear: bare-metal infrastructure was the system, and everything running inside that system was the software. Modern systems, and their many higher-order abstractions, have made that distinction somewhat less clear. Let’s start with some definitions.

For these purposes, *software* is the code you are actively developing that runs a production service delivering value to your customers. Software is what your business *wants* to run to solve a market problem.

*System* is an umbrella term for everything else about the underlying infrastructure and runtime that is necessary to run that service. Systems are

what your business *needs* to run in order to support the software it *wants* to run. By that definition, the system (or infrastructure, as we could use the two terms interchangeably here) includes everything from databases (e.g., MySQL or MongoDB) to compute and storage (e.g., containers or virtual machines) to anything and everything else that has to be provisioned and set up before you can deploy and run your software.

The world of cloud computing has made these definitions somewhat difficult to nail down, so let’s drill down further. Let’s say that to run your software, you need to run underlying components like Apache Kafka, Postfix, HAProxy, Memcached, or even something like Jira. If you’re buying access to those components as a service, they don’t count as infrastructure for this definition; you’re essentially paying someone else to run it for you. However, if your team is responsible for installing, configuring, occasionally upgrading, and troubleshooting the performance of those components, that’s infrastructure you need to worry about.

Compared to software, everything in the system layer is a commodity that changes infrequently, is focused on a different set of users, and provides a different value. Software—the code you write for your customers to use—is a core differentiator for your business: it is the very reason your company exists today. Software, therefore, has a different set of considerations for how it should be managed. Table 9-1 provides a comparison.
