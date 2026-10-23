![](bbox=[0.14, 0.08, 0.86, 0.29])

Figure 12-7. Adjusting the curve to control the animation pacing and duration

# Discussion

An animation curve, used here, stores a collection of keyframes that can be evaluated over time.

![](bbox=[0.17, 0.42, 0.26, 0.5])

> “Evaluating” in this context means sampling from the curve at a requested time in the curve: we’re asking the curve to tell us what’s going on at the specific time for which we’re evaluating.

Using an animation curve for this instead of, say, incrementing the rotation every frame is a much more powerful and flexible way to animate. It removes much of the manual work, and allows us to focus on the desired movement of the UI, rather than coding complex animations.

As we’ve mentioned throughout this cookbook, pretty much any object, including objects on the canvas, can be animated (positioned, rotated, and so on) like everything else in Unity. You can also animate the properties of these objects! (Again, just like everything else in Unity.)

# 12.4 Creating a List of Items

# Problem

You want to create a scrolling list of UI items.

# Solution

Lists are a common UI element. You can use them for everything from menus to groups of items in an inventory.