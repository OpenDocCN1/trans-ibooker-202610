
### 9.3.4 Correlation
One of the key challenges, as well as promises, in the developer observability space is correlation. Naturally, one would expect to jump from other signal types, such as a distributed trace or metric, to profiles (be that label-based or via frontend support, as with a Grafana plug-in). This is an area of active research, and we can expect more to happen here. We will cover the topic in greater detail in chapter 11.

### 9.3.5 Standards
We already discussed standards a couple of times; the gist is that you want to make sure the tools you are using to achieve developer observability are built atop and use open standards, including OpenTelemetry, for signals on the wire, as well as up-and-coming standards, like `pprof`, for representing profiles on the wire.

### 9.3.6 Using tooling in production
You can use continuous profiling in development environments as well as in production environments. There, you will need to come up with a strategy concerning the impact on live traffic versus usefulness to understand what is going on. The challenge here is twofold:
*   To get meaningful insights, you need to load debug symbols, which can and will change the execution times and cause higher resource usage.
*   Then, there is the overhead of tracking, ingesting, and storing the profiles (e.g., in `pprof` format). For that, you will need another backend that is able to store profiles efficiently and allows you to query them or one of your existing backends to introduce support for profile handling.

What we see in real life so far is that CP is validated in prod, but there are few best practices to share. Therefore, you would need to decide on a case-by-case basis whether the returns from CP outweigh the investments required (in terms of storage, ingestion, latency impact, etc.).

## Summary
*   Developer observability means using telemetry signals to equip developers with actionable insights to accelerate development; debug code; or understand the impact of a new feature, in terms of runtime or resource usage.
*   You can use continuous profiling to understand the current performance and resource usage of your service as well as compare resource usage with previous versions of your service.
*   The protobuf-encoded `pprof` format is the closest we have to a standard for representing profiles on the wire or at rest.
*   Flame graphs render call stacks in a visual manner, and eBPF is a Linux kernel extension mechanism used for profile collection across many continuous profiling tools.
*   To automate the collection of profiles from either your app code or the operating system, you can use eBPF, increasingly gaining traction.
*   Open source continuous profiling tools are a relatively recent development and include Parca, Pixie, and Pyroscope.
*   Cloud providers and observability vendors alike provide commercial offerings in the continuous profiling space.
*   Developer productivity tools, usually based on OpenTelemetry, provide developers with insights on code changes from a performance and resource usage view.
*   There are a few challenges, including a lack of interoperability and the risk of small players being acquired or discontinuing an offer; however, the overall benefit of developer productivity tooling is clear and will likely go mainstream.
*   You can benefit from continuous profiling not only in dev environments but also in prod, if you know the impact on the system.
*   Support for open standards is crucial. You will have to do your due diligence to make sure your vendor (or open source project) of choice is fully committed to supporting their offering.

# 10 Service level objectives
This chapter covers
*   What we mean by service level objectives
*   How to implement service level objectives
*   The tooling available in the SLO space
*   Considerations for implementing SLOs

At this point in the book, you should have a good idea about what *ROI-driven observability* means. There is, however, an operations topic we haven’t discussed yet and that is needed to complete the picture: how satisfied is the consumer of a service, and how do we know whether the consumer is satisfied, based on data? The consumer doesn’t have to be an external customer, especially in larger organizations, where consumers could be different business units. Now, don’t get me wrong—there’s nothing more motivating than a snarky tweet or a thoughtful comment on the orange site (aka Hacker News). However, wouldn’t it be nice if we could automate the whole process?

If you step back, you will find that DevOps and site reliability engineering (SRE) took off in the past decade, with the former being more bottom up and the latter clearly being driven by Google. The core concepts and ideas in this chapter are, indeed, a Google invention, and if you want to study every last detail, including best practices, I encourage you to head over to the Google SRE books site ([https://sre.google/books/](https://sre.google/books/)) and read everything. In this chapter, we will take a more practical approach, covering the fundamentals quickly and then showing how to use them.

The main objective is to make the availability of a service measurable and compare the metrics derived from services with goals. Once you have the goals agreed upon and you are in a position to automatically measure them, you can do a lot more:
*   You can draw nice charts and look at them, which is good for usage in frontends.
*   You can alert if you’re at risk of breaching a goal (see also our conversations in chapter 7 on cloud operations).
*   You can use these metrics as an input in a contract with your customer to promise something, and in case you are not able to deliver what you promised, you can commit to a financial compensation (possibly credits or even penalty payments). This means you’re earning trust from your customer in a quantifiable and systematic manner.
*   You can drive compliance (e.g., in the financial or health care domain).

Note
There is a parallel to the ideas we discuss in this chapter in real life: when one of our kids and I agree there will be a reward for, say, a clean room, we first need to define and agree on what *clean* means (that would be the contractual piece). I might say, for example, that over a month’s time, I expect to only find three pieces of dirty clothes on the floor (that’s the goal the child needs to reach to qualify for the reward). Then, I would randomly inspect the room and note how many dirty clothes I find on the floor (the measurement) and, at the end of the month, compare with the goal. If I were to find three or fewer pieces of clothes on the floor, I would declare the goal successfully completed, and the child would get their reward.

At any point in time, the child could look at my list and see how many pieces of clothes (in the measurement period here, which is one month) I found. With that, they could determine how close they were to breaching the agreed-upon goal or, in other words, how much wiggle room they had left (you could call that a *budget*, and indeed, that is the official term for it: an *error budget*).

In this chapter, we will first tackle service level objectives on a theoretical level; however, I’m afraid there is a bit of terminology we need to untangle first. Then, we will shift gears and have a look at tooling available in this space, and you will complete a hands-on session of implementing availability metrics and comparing with the goals you defined. In the last section, we will discuss some important considerations and provide further reading.

## 10.1 The fundamentals of SLOs
