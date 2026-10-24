+   - It is difficult to design a series of tests that go deeper than merely checking that an appropriate combination of inputs gives rise to an expected output and does not give rise to an unexpected output.
+   - It is almost impossible to verify the soundness and segregation of systems and to detect any unexpected interactions.
+   - It is difficult to determine the correct operation of the whole system, not simply the fact that functions have been performed correctly, but in understanding the mechanics of this.
+   - It is very possible that serious issues will remain dormant until a particular combination of events, demands, data structure, and software instructions leads to their initiation.

This all leads to uncertainty in the complete design unless something is done to improve the visualisation and understanding of the design, its implementation, and the results of testing.

# 11.3 Complexity

It certainly appears that modern aircraft systems have become more complex, both as individual systems and as total systems. Some of the reasons for this include the following:

+   - Advances in technology have made it easier to implement more functions in software in a high-speed processing architecture.
+   - The desire for engineering advancement: the simple need for engineers and designers to incorporate new functions and to apply new technology.
+   - The need for more automation in a bid to reduce crew workload.
+   - A desire for autonomous operations for unmanned vehicles and in the event of further reductions in flight deck crew complement.
+   - The desire to get more functional performance from fewer items of equipment.

As computing technology makes it easier to include new functions, so customer requirements are enhanced by the results of this and their needs become more complex. This may be spurred on by pilots flying new types and learning new techniques, thus making more demands based on their experience. Engineers are usually more than happy to entertain this scenario as their natural inclination is to apply new technologies.

This desire is not solely restricted to the flying side of operations. Airline and military fleet maintenance and logistics support are also under pressure to improve turnaround times. One method of doing this is to improve the accurate location of failures and to provide advance notification to the ideal repair and replacement bases. This has led to the introduction of more sophisticated algorithms to enable monitoring and on-board analysis together with data link download of information. It has also led, incidentally, to scenarios in which faults can be ‘carried’ in redundant systems until the most cost-effective repair base is reached.

This has the result of making the systems associated with control of the aircraft and mission become closely connected to the maintenance function, further increasing complexity.