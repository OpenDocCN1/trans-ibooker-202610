# Discussion

The Fresnel (pronounced “fray-nell”) effect, which this shader exploits, is the observation that the amount of light reflected off a surface varies depending on the angle of the surface to the viewer. When the angle is shallow—which is the case when the direction that the surface is facing approaches a direction perpendicular to the camera—then more reflection can be seen. In this shader, we simply take the value of how “reflective” the surface would be at any given point, and instead of actually using it for reflectance, we send the value to the Emission slot. It’s not physically realistic, but the effect can be quite pleasing.

If you set an HDR color by setting the Color property’s Mode to HDR, and combine it with a camera set up with the Glow post-processing effect (see Recipe 6.6), the glow will appear more realistically bright (Figure 6-15), because very bright objects tend to have a blooming effect when they interact with a lens (both in a camera and in the human eye).

![](bbox=[0.14, 0.38, 0.85, 0.84])

Figure 6-15. The glow effect, applied to an object in the scene