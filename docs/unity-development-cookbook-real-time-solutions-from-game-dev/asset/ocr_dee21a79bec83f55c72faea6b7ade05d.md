+   7. Create a new parameter by going to the Parameters tab and clicking the + button. Click Trigger in the menu that appears. Name the new trigger “Grab.”

+   8. Create a new layer by going to the Layers tab and clicking the + button. Name the new layer “Arms.” Click the Gear icon, and set the weight to 1.

+   9. Create a new state by right-clicking in the controller and choosing Create State → Empty. Name this new state “Passive.”

+   10. Drag and drop the IdleGrab_Neutral animation into the Animator.

+   11. Right-click the Passive state, and choose Make Transition. Select the IdleGrab_Neutral animation, and a new transition will be made from Passive to IdleGrab_Neutral. Repeat the process in the other direction: make a transition from IdleGrab_Neutral to Passive.

+   12. Select the transition from Passive to IdleGrab_Neutral, and in the Inspector, turn off Has Exit Time. This will make the transition happen immediately.

+   13. While you’re here, add a new condition to the transition by clicking the + button in the Conditions area and selecting the Grab parameter.

What we’ve done here is set up a layer that contains two animations: an empty animation named Passive, and an animation in which the character performs a grabbing motion. The grabbing motion is performed when the Grab trigger is used.

However, there’s a problem: the IdleGrab_Neutral animation affects not just the arm, but the whole body. This means that we can’t blend a grabbing animation with anything else, like walking. To make it so that the grabbing animation affects only the arm, we use an avatar mask to make the layer apply only to certain body parts:

+   1. Create a new avatar mask by opening the Assets menu and choosing Create → Avatar Mask.