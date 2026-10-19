blocking operations, optimize hot code paths, leverage concurrency, and measure. Node.js has matured to a point where it powers large-scale systems, and part of that success is due to engineers applying performance best practices to Node just as they would to any other server platform. A holistic performance strategy looks at both client and server. Your frontend optimizations could be undercut by a slow backend and vice versa—so as you optimize your web app, consider profiling and improving both ends of the stack where needed.

# Conclusion

The cost of JavaScript is something every user pays, in time or device resources, and it’s our job as engineers and developers to spend those bytes and cycles wisely on their behalf.

To recap a few key themes from this chapter:

*Know your constraints*

Not all users have high-end devices or fast networks. CPUs aren’t getting dramatically faster year over year for single-thread performance, and the gap between cheap and expensive devices is huge. Network latency is a fundamental limit that requires us to minimize round-trips and payload sizes. Browsers execute JavaScript on a single main thread, making well-timed, modular work crucial. Keeping these constraints in mind grounds our performance work in real user context.

*Load less, do less, as late as possible*

This mantra underpins many techniques. Code splitting and lazy loading help you load less code up front. Scheduling, idle work, and efficient algorithms help your code do less work for the same result. Deferring noncritical tasks and resources means doing things as late as possible and no later—you still need to meet users’ needs on time. By being mindful of *when* and *how much* JavaScript runs, you can create more