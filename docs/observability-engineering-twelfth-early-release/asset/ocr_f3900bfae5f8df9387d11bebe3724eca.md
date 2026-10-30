# NOTE

Waterfall visualizations show how an initial value is affected by a series of intermediate values, leading to a final cumulative value.

# Trace ad784a29-0554-460a-bfc2-defb6de57e97

![](bbox=[0.14, 0.26, 0.86, 0.65])

Figure 6-1. This waterfall-style trace visualization displays four trace spans during one request

Each chunk of this waterfall is called a trace span, or span for short. Within any given trace, spans are either the root span—the top-level span in that trace—or are nested within the root span. Spans nested within the root span may also have nested spans of their own. That relationship is sometimes referred to as parent-child. For example, in Figure 6-2, if Service A calls Service B, which calls Service C, then for that trace, Span A is the parent of Span B, which is the parent of Span C.