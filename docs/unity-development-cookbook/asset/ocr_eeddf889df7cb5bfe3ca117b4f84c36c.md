```
// Lock the cursor to the middle of the screen or window.
Cursor.lockState = CursorLockMode.Locked;

// Prevent the cursor from leaving the window.
Cursor.lockState = CursorLockMode.Confined;

// Don't restrict the mouse cursor at all.
Cursor.lockState = CursorLockMode.None;

// Hide the mouse cursor.
Cursor.visible = false;
```

# Discussion

In the editor, you can press the Escape key to re-enable the mouse cursor and free it from cursor jail.

# 3.5 Responding to Mouseover and Click Events

# Problem

You want to detect when users move a mouse over an object, and when they click it.

# Solution

To detect this, you can use the Event System, which is capable of tracking the position of a cursor on the screen and detecting when it's over an object in the scene. To begin working with the Event System, create an object with an EventSystem component attached, via the GameObject menu:

+   1. Open the GameObject menu, and select UI → EventSystem. A new EventSystem object will be created.

![](bbox=[0.2, 0.66, 0.27, 0.75])

If you create a Canvas, you'll also create an EventSystem at the same time, because the UI system relies on the same techniques as we're using in this recipe.