CSS or JavaScript is slow, it can block rendering or interaction readiness.

# Parsing and script execution

Large JavaScript bundles take time to download, parse, and execute. If the main thread is busy executing a script when the user interacts, that interaction will be delayed, affecting INP. If scripts block the parser for too long, they will delay when content appears, affecting LCP.

# Layout and paint complexity

Deeply nested styles or very complex layouts with thousands of DOM nodes can slow down style calculation and layout, which can cause janky interactions or slow rendering. This affects INP and potentially cumulative layout shift (CLS), if things shift slowly. Large repaints, like redrawing a big canvas or reflowing a giant table, can also slow down frame rendering.

# Layout thrashing

If JavaScript repeatedly forces layout calculations in a loop (for example, reading DOM size, then modifying DOM, then reading again), the browser may do multiple style and layout passes where one would suffice. This can massively degrade performance.

# Unscheduled background work

Scripts like those for analytics or ads often do their work on timers or after load. If these run without care, they can steal precious main-thread time. For example, an analytics script doing heavy computation right as the page is becoming interactive could interfere with the user’s first interaction.

# Visual stability issues

If content is inserted dynamically without reserving space, the user experiences layout shifts (poor CLS). This might not slow down the