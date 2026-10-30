That begs a question around how much intelligence should be applied to this number crunching. Is artificial intelligence and machine learning the inevitable solution to all our debugging problems?

# This Misleading Promise of AIOps

Since Gartner coined the term in 2017, *artificial intelligence for operations (AIOps)* has generated lots of interest from companies seeking to somehow automate common operational tasks. Delving deeply into [the misleading promise of AIOps]( ) is beyond the scope of this book. But AIOps intersects observability, reducing alert noise and anomaly detection.

In [Chapter 12]( ), we unpack a simpler approach to alerting that makes the need for algorithms to reduce alert noise a moot point. The second intersection, anomaly detection, is the focus of this chapter, so we’ll unpack that in this section.

As seen earlier in this chapter, the concept behind using algorithms to detect anomalies is to select a baseline of “normal” events and compare those to “abnormal” events not contained within the baseline window. Selecting the window in which that’s done can be incredibly challenging for automated algorithms.

Similar to using BubbleUp to draw a box around the area you care about, AI must decide where to draw its own box. If a system behaved consistently over time, anomalies would be worrisome, unusual cases that could be easily detected. In an innovative environment with system behavior that changes frequently, it’s more likely that AI will draw a box of the wrong size. The box will be either too small—identifying a great deal of perfectly normal behavior as anomalies, or too large—miscategorizing anomalies as normal behavior. In practice, both types of mistakes will be made, and detection will be far too noisy or far too quiet.

This book is about the engineering principles needed to manage running production software on modern architectures with modern practices. Any reasonably competitive company in today’s world will have engineering