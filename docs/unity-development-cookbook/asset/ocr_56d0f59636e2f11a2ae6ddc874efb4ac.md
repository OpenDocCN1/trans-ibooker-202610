# Discussion

The player’s speakers or headphones play audio when an AudioSource component that’s currently playing audio is in range of an AudioListener component. When you create a new scene, the main Camera object that’s included in all objects comes with an AudioListener component attached.

![](bbox=[0.15, 0.22, 0.24, 0.3])

> You should only have one AudioListener in the scene at a time. If you have more than one, Unity will warn you, and only one of them will work.

# 11.2 Setting Up a Mixer

# Problem

You want to manage the levels and settings for the audio being played by your project in a central location.

# Solution

You can manage the volume levels of different groups of audio sources by creating an audio mixer asset and setting up groups in the mixer. You can configure an audio source to route its audio to a specific group; each group can have its own volume level and effects.

To create an audio mixer:

+   1. Open the Assets menu, and choose Assets → Create → Audio Mixer. A new audio mixer asset will be created; rename it to whatever you like. (“Audio Mixer” is common; you might not need more than a single mixer in your project.)
+   2. Double-click the new asset. The Audio Mixer panel will open (Figure 11-3).
+   3. The audio mixer manages a collection of groups. Groups have a volume slider and can have effects attached to them. They can also have children, which means that changing the volume of the parent affects the volume of its children. There’s always one group, called Master; all other groups are children of Master.
+   4. Create a new group by clicking the + button at the right of groups. (For example, you can create a group called “Music,” and another called “Sound Effects.”)