```
[SerializeField] AnimationCurve curve = \n AnimationCurve.EaseInOut(0, 0, 1, 1);
```

> # NOTE\n\nWhen you use the SerializeField attribute, you're letting Unity know that the contents of those variables should be saved to disk and displayed as an editable value in the Inspector.

+   4. We'll need to add a public method to start a coroutine, calling the StartSpinning method that we just created:

```
public void Spin() {\n // Start a spin.\n StartCoroutine(StartSpinning());\n}
```

> # TIP\n\nWhen a coroutine is started, it pauses execution and returns control to Unity every time the yield return statement is encountered. If you need a refresher on coroutines, check out Chapter 2.

+   5. Attach a SpinButton component to the button we created earlier.

+   6. Click the + button in the On Click field.

+   7. Drag the button itself into the field.

+   8. In the pop-up menu, choose SpinButton → Spin.

+   9. Run the game. When you click the button, it will spin around! A masterpiece.