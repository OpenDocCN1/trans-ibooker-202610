```
transform.localRotation = Quaternion.identity;
}
```

This code defines a method, StartSpinning, that we’ll be calling as a coroutine, and which updates the rotation of the object it’s attached to.

+   3. We’ll also need to define some variables for the parameters of the rotation. Add the following to the method we just created:

```
// The amount of time needed to perform a full spin
[SerializeField] float spinTime = 0.5f;

// Controls the pacing of the animation.
[SerializeField] AnimationCurve curve =
    AnimationCurve.EaseInOut(0, 0, 1, 1);
```

![](bbox=[0.2, 0.36, 0.28, 0.47])

When you use the SerializeField attribute, you’re letting Unity know that the contents of those variables should be saved to disk and displayed as an editable value in the Inspector.

+   4. We’ll need to add a public method to start a coroutine, calling the StartSpinning method that we just created:

```
public void Spin() {
    // Start a spin.
    StartCoroutine(StartSpinning());
}
```

![](bbox=[0.2, 0.64, 0.28, 0.74])

When a coroutine is started, it pauses execution and returns control to Unity every time the yield return statement is encountered. If you need a refresher on coroutines, check out Chapter 2.

+   5. Attach a SpinButton component to the button we created earlier.
+   6. Click the + button in the On Click field.
+   7. Drag the button itself into the field.
+   8. In the pop-up menu, choose SpinButton → Spin.
+   9. Run the game. When you click the button, it will spin around! A masterpiece.

You can also adjust the curve to control the pacing of the animation, as well as its duration, using the variables we exposed to the editor, as shown in Figure 12-7.