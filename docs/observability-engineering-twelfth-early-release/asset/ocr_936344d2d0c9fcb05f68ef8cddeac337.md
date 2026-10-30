# # Part II. Fundamentals of
# Observability

In the first part of this book, we examined the definition of “observability,” its necessity in modern systems, its evolution from traditional practices, and the way it is currently being used in practice. This second section delves deeper into technical aspects and details why particular requirements are necessary in observable systems.

[Chapter 5](#) introduces the basic data type necessary to build an observable system—the arbitrarily wide structured event. It is this fundamental data type for telemetry that makes the analysis described later in this part possible.

[Chapter 6](#) introduces distributed tracing concepts. It breaks down how tracing systems work in order to illustrate that trace data is simply a series of interrelated arbitrarily wide structured events. This chapter walks you through manually creating a minimal trace with code examples.

[Chapter 7](#) introduces the OpenTelemetry project. While the manual code examples in [Chapter 6](#) help illustrate the concept, you would more than likely start with an instrumentation library. Rather than choosing a proprietary library or agent that locks you into one vendor’s particular solution, we recommend starting with an open source and vendor-neutral instrumentation framework that allows you to easily switch between observability tools of your choice.

[Chapter 8](#) introduces the core analysis loop. Generating and collecting telemetry data is only a first step. Analyzing that data is what helps you achieve observability. This chapter introduces the workflow required to sift through your telemetry data in order to surface relevant patterns and quickly