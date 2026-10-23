![](bbox=[0.14, 0.08, 0.84, 0.43])

Figure 6-28. The light probes used in Figure 6-27

# Discussion

Objects that don’t contribute to global illumination can’t receive baked light, because the light doesn’t really “exist” during gameplay—instead, the light is written onto textures, and the static objects read from the texture to simulate light falling on them. This means that a real-time-lit object can look out of place with its surroundings, because absent any lighting information, it will be lit only by the global environment (such as the skybox, or ambient light sources). To solve this, you could create a real-time light for every baked light, or set every light to be Mixed (meaning it effectively functions as a baked light and a real-time light), but that can be a performance hit—especially when you have lots of lights, and you’re rendering on a low-powered device that can’t use deferred rendering (a rendering technique that significantly reduces the cost of multiple lights).

![](bbox=[0.17, 0.73, 0.24, 0.8])

> We don’t cover deferred rendering in this book, but you can learn more in the Unity documentation.

Light probes are a solution to this problem and allow your real-time-lit objects to approximate the lighting conditions they would have experienced had the lighting been baked. A light probe does this by measuring the amount of baked light