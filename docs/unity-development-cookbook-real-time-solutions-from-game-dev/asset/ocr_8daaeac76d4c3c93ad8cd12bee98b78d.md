normal range but at double the rate; and the dotted line, 2x the sine of time (y = 2 * sin(t)), oscillates at a wide range and at a normal rate.

![](bbox=[0.14, 0.153, 0.86, 0.363])

# Figure 6-19. Oscillation ranges and rates

# 6.15 Using a Subgraph to Reuse Graph Components

# Problem

You want to simplify your shader graphs by reusing common patterns of nodes.

# Solution

Use a subgraph, which is a collection of nodes that you can store in a file and reuse in other graphs. Subgraphs appear as a single “node” in your graphs.

To create a subgraph, follow these steps:

+   1. Open the Assets menu, and choose Create → Shader Graph → Sub Graph.

+   2. Open the Graph Inspector by clicking the Graph Inspector button at the top right of the Shader Graph window.