```
{
    _isSpinning=true;

    // Make a double animation object, and register
    // with the Completed event.
    var dblAnim=new DoubleAnimation();
    dblAnim.Completed +=(o, s)=> { _isSpinning=false; };

    // Button has four seconds to finish the spin!
    dblAnim.Duration=new Duration(TimeSpan.FromSeconds(4));

    ...
}
}
```

With this adjustment, you should have a fighting chance of clicking the button while it is spinning, at which point it will fade away.

> Note The BeginTime property of an Animation class also takes a TimeSpan object. Recall that this property can be set to establish a wait time before starting an animation sequence.

# Reversing and Looping an Animation

You can also tell Animation objects to play an animation in reverse at the completion of the animation sequence by setting the AutoReverse property to true. For example, if you want to have the button come back into view after it has faded away, you could author the following:

```
private void btnSpinner_OnClick(object sender, RoutedEventArgs e)
{
    DoubleAnimation dblAnim=new DoubleAnimation
    {
        From=1.0,
        To=0.0
    };
    // Reverse when done.
    dblAnim.AutoReverse=true;
    btnSpinner.BeginAnimation(Button.OpacityProperty, dblAnim);
}
```

If you would like to have an animation repeat some number of times (or to never stop once activated), you can do so using the RepeatBehavior property, which is common to all Animation classes. If you pass in a simple numerical value to the constructor, you can specify a hard-coded number of times to repeat. On the other hand, if you pass in a TimeSpan object to the constructor, you can establish an amount of time the animation should repeat. Finally, if you want an animation to loop ad infinitum, you can simply specify