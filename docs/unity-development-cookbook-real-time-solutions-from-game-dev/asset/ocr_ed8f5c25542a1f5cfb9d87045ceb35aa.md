We now need to configure this transition so that it applies when the Speed parameter is above a certain threshold:

+   1. Select the line, and go to the Conditions list at the bottom of the Inspector.
+   
+   2. Click the + button, and a new condition will be added. There’s only one parameter in the controller, so it will automatically be selected. Make sure that the mode is set to Greater, and set the value to 0.1. This means that this transition will apply when the Speed parameter is greater than 0.1.
+   
+   3. Turn off the Has Exit Time setting. This means that this transition can apply at any time, and doesn’t need to wait for the current animation to complete.

When you’re done, the Inspector should look like Figure 8-6.