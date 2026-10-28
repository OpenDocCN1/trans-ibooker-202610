To further simplify `run_game()`, we’ll move the code for updating the screen to a separate method called `_update_screen()`.

# **alien_invasion.py**

```
def run_game(self):
    """Start the main loop for the game."""
    while True:
        self._check_events()
        self._update_screen()
        self._clock.tick(60)


def _check_events(self):
    --snip--


def _update_screen(self):
    """Update images on the screen, and flip to the new screen."""
    self.screen.fill(self.settings.bg_color)
    self.ship.blitme()

    pygame.display.flip()
```

We moved the code that draws the background and the ship and flips the screen to `_update_screen()`. Now the body of the main loop in `run_game()` is much simpler. It’s easy to see that we’re looking for new events, updating the screen, and ticking the clock on each pass through the loop.

If you’ve already built a number of games, you’ll probably start out by breaking your code into methods like these. But if you’ve never tackled a project like this, you probably won’t know exactly how to structure your code at first. This approach gives you an idea of a realistic development process: you start out writing your code as simply as possible, and then refactor it as your project becomes more complex.

Now that we’ve restructured the code to make it easier to add to, we can work on the dynamic aspects of the game!