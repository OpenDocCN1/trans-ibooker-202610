![](bbox=[0.14, 0.08, 0.86, 0.34])

Figure 6-21. The subgraph in use within another shader

# 6.16 Implementing a Dissolve Effect Using a Shader Graph

# Problem

You want to create an effect in which parts of the object gradually disappear. You want the edges of the effect to glow.

# Solution

Follow these steps to construct a shader in the Shader Graph tool that achieves this effect:

+   1. Create a new Lit URP shader graph by following the instructions in Recipe 6.10. Name the new shader graph “Dissolve.”

+   2. Open the Graph Inspector, and go to the Graph Settings tab. Scroll down to Surface Type, and change it from Opaque to Transparent. Next, turn on Alpha Clipping.