5. Next, we’ll also bind the action to a button on a gamepad controller. In the actions list, click the + button for Change Colour, and choose Add Binding. A new empty binding will appear.

6. Select the new binding, and set its Path to Gamepad → Button South.

**NOTE**

Just about every modern game controller has four buttons on its righthand side, in a diamond shape. These are often referred to as the *face buttons*. The *South* button represents the bottom of this diamond. Different controllers refer to these buttons by different names: the south button is the X button on a PlayStation controller, the A button on an Xbox controller, and the B button on a Nintendo Switch controller. Much like how By Location of Key works for specifying the position of a control rather than the name of a control, specifying “south button” lets you refer to the position of the button on a controller.

7. Set the binding’s Use In Control Scheme to Gamepad.

**TIP**

If you don’t have a gamepad available to use, try binding it to something else (for example, the left mouse button).

8. Click Save Asset, and close the window.

Now that we’ve created the new action and bound it to some controls, we’ll make our cube able to respond to the action:

1. Add the following method to *CubePlayer.cs*:

```
public void OnChangeColour(InputAction.CallbackContext context) {
    // When this action is performed, change color.
    if (context.performed) {
        ChangeColour();
    }
}
```