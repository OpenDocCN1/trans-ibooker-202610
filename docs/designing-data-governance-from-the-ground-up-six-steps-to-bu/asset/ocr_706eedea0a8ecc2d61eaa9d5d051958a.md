users faster. For instance, having a designer serve on the council allows them to conduct user interviews and own the qualitative data from those interviews. This includes sharing the interview transcripts with other council members and storing them in your shared communications tool.

# Give Your Data Stewards Room to Fail

Remember, you're in the governance-driven development stage, not the production stage. Once you've progressed to production, the time for experiments is done. Until then, use development as your sandbox to learn what does and doesn't work for your data project.

This is especially crucial if you're using new or immature tech. Managing big data demands a lot of moving parts under the hood. Xu describes this challenge when explaining how Netflix transitioned its data platform into containerized cloud infrastructure:

> "There are hundreds of thousands of physical machines powering the cloud in each data center underneath the hood. At this scale, hardware failures are inevitable. When these failures arise unexpectedly, it could be challenging for the systems to keep up with the availability and consistency expectations. It's even more challenging in an unbounded low latency stream processing environment where any failure recovery can result in backpressure buildup. Cloud-native resilience for a streaming-first architecture would mean significant engineering challenges."

How did Xu surmount these engineering challenges? Instead of avoiding them, he designed the system to monitor, detect, and tolerate them. If this sounds a lot like DevOps, you're correct.

Remember when you asked yourself what's the worst that could happen while writing your roadmap in Chapter 4, Write Your Data Roadmap, on page 43? Revisit that worst case scenario now. If you are truly breaking ground with your project, I guarantee that you'll fail several times in both expected and surprising ways. Xu and his team certainly did.

On launch day at Netflix, the Kafka cluster his team built proved to be too big. Once this cluster reached its scaling limit, it couldn't recover after one of 200+ brokers died. This caused mass data loss across Netflix, even though Xu's team had correctly predicted the right amount of traffic. Thanks to the psychological safety on his team, he knew they could recover from this mishap. He also knew that this mistake was too dire to occur again.⁴

4. https://kafka.apache.org/intro