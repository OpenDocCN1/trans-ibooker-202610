original. (Consider making it a child of the original object.)

+   2. Turn off Contribute GI so that it casts real-time shadows.

+   3. Remove every component except the MeshRenderer.

+   4. Set the MeshRenderer's Cast Shadows property to Shadows Only.

This means that it won't appear in the scene, but it will cast shadows onto other objects.

# Discussion

Another solution is to use light probes. Place light probes in areas both inside the shadowed region and outside; this will darken objects that pass into the shadowed area, though you won't see the edges of the shadow (it will just darken uniformly). It doesn't look as good, but it's cheaper to render.

> **NOTE**
> 
> The alternative is to design your scene layouts so that static objects don't cast shadows onto a real-time object in the first place.

# 6.20 Using Light Probes to Influence Lighting

# Problem

You want to use light probes, so that real-time objects are lit by light bouncing off objects with baked lighting.

# Solution

Create a light probe group by opening the GameObject menu and choosing Light → Light Probe Group. This creates a group of probes; you can select each individual probe and reposition it.