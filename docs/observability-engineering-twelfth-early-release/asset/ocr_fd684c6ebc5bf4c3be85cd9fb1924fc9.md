timestamp, duration, parent ID, trace ID, name, type, tags, and a special span type field. Like a trace, a causal graph is a directed acyclic graph of SpanEvents. Giving engineers the ability to easily produce and analyze traces outside of traditional instrumentation opens up a lot of new avenues, such as instrumenting CI/CD systems, as described in Chapter 11.

To support the causal graph model, we have built custom tracing libraries for languages like PHP/Hack, JavaScript, Swift, and Kotlin. In addition to those libraries, we leverage open source tracing libraries for Java and Go.

![](bbox=[0.125, 0.3, 0.875, 0.795])

Figure 18-5. Slack’s tracing infrastructure, with applications in pink, receivers and exporters in blue

Our Hack applications are instrumented with an OpenTracing-compatible tracer. Our mobile and desktop clients trace their code and emit SpanEvents using either a high-level tracer or a low-level span-creation API. These