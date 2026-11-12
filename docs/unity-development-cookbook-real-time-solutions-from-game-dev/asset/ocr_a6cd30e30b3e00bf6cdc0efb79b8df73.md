Light probes are affected by both real-time and baked lighting. Real-time objects will use nearby light probes to calculate how they should be lit. In Figure 6-27, there are no real-time lights in the scene; instead, light probes near the light source capture how light affects objects at each point, and the cylinder is using that information to light itself. Figure 6-28 shows the light probes set up here.

![](bbox=[0.145, 0.245, 0.855, 0.66])

Figure 6-27. A real-time cylinder, lit by a baked emissive object