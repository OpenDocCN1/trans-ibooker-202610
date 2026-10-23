![](bbox=[0.13, 0.09, 0.86, 0.33])

Figure 6-19. Oscillation ranges and rates

# 6.15 Using a Subgraph to Reuse Graph Components

# Problem

You want to simplify your shader graphs by reusing common patterns of nodes.

# Solution

Use a subgraph, which is a collection of nodes that you can store in a file and reuse in other graphs. Subgraphs appear as a single “node” in your graphs.

To create a subgraph, follow these steps:

+   1. Open the Assets menu, and choose Create → Shader Graph → Sub Graph.
+   2. Open the Graph Inspector by clicking the Graph Inspector button at the top right of the Shader Graph window.
+   3. Add outputs to the Output node by clicking the + button and creating new outputs.
+   4. Construct your nodes just as if you were making a shader. The difference is that, rather than the output being sent to a Fragment node to perform lighting calculations, the result will be exposed as outputs of the node when the subgraph is embedded in another shader. When you drag and drop the output of a node onto a slot, the slot will take on the type of the incoming value.