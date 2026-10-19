# Chapter 6. Trade-Offs in Performance Optimizations

> *Web development is a series of trade-offs.*
> 
> — Tim Kadlec

Every performance enhancement comes with a cost: increasing complexity, adding maintenance, even potentially hurting some other metric. This chapter takes a pragmatic look at strategies for improving JavaScript performance—and the trade-offs they entail. The goal is to help you evaluate optimizations objectively, balancing explicit recommendations with context. As Tim Kadlec wisely points out, there is rarely a one-size-fits-all solution in web development; we operate in a “grey goo” of possibilities. You have to know your priorities (speed, accessibility, developer productivity, etc.) and choose your optimization strategies accordingly.

# Balancing Different Performance Metrics

As you’ve seen throughout this book, performance itself isn’t one metric—it’s many. Sometimes you need to trade one off for another. For example, to improve largest contentful paint (LCP), you might delay some script that is needed for interactivity, which could hurt interaction to next paint (INP) if the user tries to interact quickly. Or a strategy to optimize time to interactive (TTI) might increase the total load time slightly. You have to decide which metrics are more important for your scenario.

For a concrete example: if you have heavy JavaScript that doesn’t affect above-the-fold content, you might postpone it until after the first paint. This improves LCP because the image or text loads faster; it might also improve INP since it leaves the main thread free at the start. However, if the user tries to interact early, some functionality might not be ready. If that happens