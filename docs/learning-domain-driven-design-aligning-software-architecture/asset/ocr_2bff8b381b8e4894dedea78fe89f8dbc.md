# Chapter 13

+   1. B: Analyze the organization's business domain and its strategy.
+   2. D: A and B.
+   3. C: A and B.
+   4. An aggregate with a bounded context-wide boundary may make all of the bounded context’s data a part of one big transaction. It’s also likely that performance issues with this approach will be evident from the get go. Once that happens, the transactional boundary will be removed. As a result, it will no longer be possible to assume that the information residing in the aggregate is strongly consistent.