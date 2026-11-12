```
}
}
```

+   2. Save the file, and return to Unity.

+   3. Select the Player Input object, and go the Inspector for its Player Input component.

> NOTE
> 
> When you created the new actions asset earlier, the Input Actions field was set to the new asset. You can change which actions asset the component is using by dragging and dropping a replacement asset into the Input Actions field.

+   4. Open the Events section, and choose Player → Change Colour.

+   5. Click the + button to add a new receiver.

+   6. Drag and drop the Cube object into the object field, and change the function to CubePlayer → OnChangeColour.

+   7. While you're here, perform the same steps you did earlier for connecting the Move action to the CubePlayer's OnMove method; the connection will have been lost when you changed which actions asset the component uses.

Enter Play Mode, and press the C key on the keyboard or the south gamepad button (or any other control you've bound to the Change Colour action). The cube will change color.

# 3.4 Locking and Hiding the Mouse Cursor

# Problem

You want to make use of mouse movement in your game, but you don't want a cursor to be shown onscreen.