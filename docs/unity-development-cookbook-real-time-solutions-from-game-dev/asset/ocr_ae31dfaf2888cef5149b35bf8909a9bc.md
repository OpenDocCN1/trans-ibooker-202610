```
moveInput = context.ReadValue<Vector2>();
}
```

+   2. Save the file and return to Unity.

+   3. Next, we'll make this method get called by the input system whenever the Move action changes state. To begin, select the Player Input object.

+   4. In the Inspector for the Player Input component, set Behaviour to Invoke Unity Events.

+   5. A new section will appear at the bottom, labeled Events. Open Events → Player.

+   6. Click the + button attached to the Move event to add a new entry.

+   7. Drag and drop the Cube object into the object slot.

+   8. Change the function drop-down from No Function to CubePlayer → OnMove.

+   9. Press Play, and use the W, A, S, and D keys to move the cube around.

With this setup, the Player Input component will call the OnMove method on the cube’s CubePlayer script. This method receives an InputAction.CallbackContext method that lets the code get information about what specific input was received. The Move action is defined as one that contains two-dimensional information (that is, left-to-right and forward-to-backward). In OnMove, we call ReadValue<Vector2> to fetch that data and store it in the moveInput variable. This variable is then used in Update to actually move the cube around.