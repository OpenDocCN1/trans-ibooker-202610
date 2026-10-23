![](bbox=[0.15, 0.09, 0.26, 0.22])

Objects that contribute to global illumination aren't allowed to move, because if they do, the lighting won't adjust to account for it. If an object needs to be able to both move around and act like it's receiving light from static objects, you'll need to use light probes; see Recipe 6.20.

# Discussion

*Baked lighting* is the process of calculating ahead of time how light bounces around an environment. Unlike real-time lighting, which has to work as quickly as possible, baked lighting has the luxury of taking as much time as it needs to—often minutes or hours. Because it has this extra time, it's able to perform path-tracing of virtual photons as they leave the light sources in your scene and bounce around the level. As these calculations are performed, textures are generated that store the amount of light that the various parts of the objects are receiving; at runtime, these textures are used to simulate the lighting.

Baked lighting tends to look better than real-time lighting (see Figure 6-24 for an example), but it's less flexible. Because the texture is generated before gameplay and takes significant time to create, it can't be modified during gameplay. If an object that casts a baked shadow moves, the shadow won't move with it.

In Figure 6-24, we see the cube on the left is real-time lit, and the cube on the right uses baked lighting. Notice how the shadow of the cube on the right is more realistic; it's sharper near the base of the cube and blurrier further away, while the real-time-lit cube's shadow has the same fuzziness at all points around the edge.

There are multiple Static modes besides Contribute GI. If you know that an object will never move—perhaps it's fixed scenery in the level—you can click the Static checkbox at the top right of the Inspector, and *all* static modes are enabled at once. This means that Unity can enable additional optimizations, including the ability to render multiple objects that share a material at the same time, in a single batch.