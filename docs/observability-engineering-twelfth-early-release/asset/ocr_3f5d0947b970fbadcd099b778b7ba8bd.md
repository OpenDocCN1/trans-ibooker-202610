This data allowed Slack to look at telemetry with context over time. This is one of many examples of using observability in day-to-day operations at Slack. You can adopt a similar approach to identify changes and breaks by embedding observability into your own investigations.

# Conclusion

This chapter illustrates how observability can be useful in the software supply chain. I shared how Slack instrumented the CI pipeline and recent examples of debugging distributed systems. The intricacies of debugging distributed systems are generally top of mind for application developers trying to understand how their code behaves in production environments. But, prior to production, other distributed systems may be equally challenging to properly understand and debug.

With the right tools and dimensionality in the software supply chain, Slack engineers were able to solve complex problems throughout the CI workflow that were previously invisible or undetected. Whether debugging complaints that an application is slow or that CI tests are flaky, observability can help developers correlate problems in high-complexity systems that interact.

> 1  Maya Kaczorowski. “Secure at Every Step: What is Software Supply Chain Security and Why Does It Matter?,” GitHub Blog, September 2, 2020.