from button import Button

Because we need only one Play button, we’ll create the button in the `__init__()` method of `AlienInvasion`. We can place this code at the very end of `__init__()`:

**alien_invasion.py**

```
def __init__(self):
    --snip--
    self.game_active = False

    # Make the Play button.
    self.play_button = Button(self, "Play")
```

This code creates an instance of `Button` with the label Play, but it doesn’t draw the button to the screen. To do this, we’ll call the button’s `draw_button()` method in `_update_screen()`:

**alien_invasion.py**

```
def _update_screen(self):
    --snip--
    self.aliens.draw(self.screen)

    # Draw the play button if the game is inactive.
    if not self.game_active:
        self.play_button.draw_button()

    pygame.display.flip()
```

To make the Play button visible above all other elements on the screen, we draw it after all the other elements have been drawn but before flipping to a new screen. We include it in an `if` block, so the button only appears when the game is inactive.

Now when you run *Alien Invasion*, you should see a Play button in the center of the screen, as shown in Figure 14-1.