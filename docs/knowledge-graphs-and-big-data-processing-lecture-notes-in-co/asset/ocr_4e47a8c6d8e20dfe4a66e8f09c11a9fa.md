# Table 1. Object-Attribute table for performing FCA.

| | Attribute 1 | Attribute 2 | Attribute 3 |
|---|---|---|---|
| Object 1 |  | X | X |
| Object 2 |  | X |  |
| Object 3 | X |  | X |

Instead of using attributes in the column of the FCA matrix, in our approach, we replace the attributes with a boolean condition C. This is the same as the context condition C used in our approach. For example, the context C from the motivating example can be broken down into C = C1 ∧ C2 where C1 = “contains property dbo:occupation”, and C2 = “has the same value for property dbo:occupation”. The execution of the FCA algorithm remains unchanged by this adaptation since the format of the input to FCA is still a binary matrix.

When applied to RDF molecules, formal concept analysis returns a set of formal concepts < M, C > where M is a set of all the molecules that contain all conditions contained in C. That is, by applying FCA, the set of molecules that satisfy a certain context condition can be obtained. Thus, the molecules that do not meet the context condition are pruned. In Fig. 7, an example of context validation is demonstrated. Edges in a bipartite graph are filtered according to a threshold value γ as detailed in the previous section. Next, the remaining edges are validated by constructing an FCA matrix according to context condition C. The FCA algorithm returns the edge satisfying the context conditions. The edges that do not satisfy the context condition are discarded.

# 4.4 The 1-1 Perfect Matching Calculator

COMET solves the problem of context-aware entity matching by computing a 1-1 weighted perfect matching between the sets of RDF molecules. The input of the 1-1 weighted perfect matching component is the weighted bipartite graph created on the previous step. Since each weight of an edge between two RDF molecules corresponds to a combined score of semantic similarity and context equivalence value, we call this a 1-1 context-aware matching calculator. The effect of this 1-1 context aware matching calculator is demonstrated in Fig. 9 Finally, a combinatorial optimization algorithm like the Hungarian algorithm [267] is utilized to compute the matching.

# 4.5 Integration Use Case: Applying Fusion Policies

In order to apply this context-aware entity matching pipeline into a data integration scenario, we envision the usage of fusion policies defined by Collarana et al. [84]. To consolidate entities identified as contextually equivalent, COMET can make use of synthesis policies, i.e. a user-supplied function that defines how the RDF molecules should be combined to form a connected whole. COMET can adopt the following synthesis policies: