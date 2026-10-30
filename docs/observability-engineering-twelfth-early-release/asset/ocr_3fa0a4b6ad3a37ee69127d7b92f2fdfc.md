until you get to the correct source of issues, without losing your train of thought (which typically means getting answers within seconds instead of minutes)?

+   - Can you answer questions like the preceding ones even if that particular issue has never happened before?
+   - Do the results of your debugging investigations often surprise you by revealing new, perplexing, and bizarre findings, or do you generally find only the issues you suspected that you might find?
+   - Can you quickly (within minutes) isolate any fault in your system, no matter how complex, deeply buried, or hidden within your stack?

Meeting all of the preceding criteria is a high bar for many software engineering organizations to clear. If you can clear that bar, you, no doubt, understand why observability has become such a popular topic for software engineering teams.

Put simply, our definition of “observability” for software systems is a measure of how well you can understand and explain any state your system can get into, no matter how novel or bizarre. You must be able to comparatively debug that bizarre or novel state across all dimensions of system state data, and combinations of dimensions, in an ad hoc iterative investigation, without being required to define or predict those debugging needs in advance. If you can understand any bizarre or novel state *without needing to ship new code*, you have observability.

We believe that adapting the traditional concept of observability for software systems in this way is a unique approach with additional nuances worth exploring. For modern software systems, observability is not about the data types or inputs, nor is it about mathematical equations. It is about how people interact with and try to understand their complex systems. Therefore, observability requires recognizing the interaction between both people and technology to understand how those complex systems work together.