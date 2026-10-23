+   1. Open the Package Manager window by choosing Window → Package Manager.
+   2. Change the Packages setting at the top left of the window from In Project to Unity Registry.
+   3. Scroll down to Input System in the list of packages, or search for Input System in the search field at the top right.
+   4. Select the Input System from the list, and click the Install button.

Unity will download and install the Input System package into your project. Unity may ask you if you want to update your project’s settings to use the new Input System; select Yes, and it will restart the Editor.

Once the Input System has been installed, you can make use of it in your code.

For example, we could use the following to re-create what we did in Recipe 3.1, but using the Input System:

```
using UnityEngine.InputSystem;

if (Keyboard.current.aKey.wasPressedThisFrame) {
    Debug.Log($"The A key was pressed!");
}
if (Keyboard.current.aKey.isPressed) {
    Debug.Log($"The A key is being held down!");
}
if (Keyboard.current.aKey.wasReleasedThisFrame) {
    Debug.Log($"The A key was released!");
}

if (Keyboard.current.anyKey.wasPressedThisFrame) {
    Debug.Log("Some key was pressed!");
}
```

# Discussion

While it’s definitely possible to directly access specific buttons, like keys on the keyboard, there’s a much better way to work with user input: Input Actions, which we’ll cover in the next recipe.

![](bbox=[0.17, 0.75, 0.25, 0.83])

One place where you might want to directly access buttons is for development purposes. For example, when you’re in the early stages of development, or testing a version of your game, it can be convenient to have special developer keys that perform special tasks in your game, like making the player immune to damage. You typically don’t expose these to the player, so there’s no reason to do the extra work in setting them up as an Input Action, and instead just access the buttons directly.