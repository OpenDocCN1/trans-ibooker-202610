# Discussion

You can also configure the Ratio and Knee parameters to adjust how the Duck Volume effect modifies its volume. Additionally, the Attack Time and Release Time settings give you control over how quickly the effect is applied, and how long it takes for it to go away.

# 11.6 Using Multiple Audio Zones

# Problem

You want to create areas in which audio sounds different. For example, you want the game to have a deep echo when the player walks into a cave.

# Solution

Use a *reverb zone* to create areas of a scene that apply different reverb settings to your audio listener. Reverb zones distort the sound heard by a listener, depending on where the listener is within the zone, and are used when you want to gradually apply an ambient effect to a place as the player enters it.

To create a reverb zone:

+   1. Open the GameObject menu, and choose Audio → Audio Reverb Zone.
+   2. Position the new zone in the area where you want the reverb to apply.
+   3. Click and drag the points on the two spheres to set the minimum and maximum radius of the effect (Figure 11-8).
+   4. In the Inspector (Figure 11-9), select what kind of effect you want to apply. There’s a wide range of presets, including concert halls, caves, padded rooms, and everything in between.
+   5. Play the project, and move the camera into the reverb zone. As you enter the zone, the effect will begin to apply to any audio being received by the audio listener.