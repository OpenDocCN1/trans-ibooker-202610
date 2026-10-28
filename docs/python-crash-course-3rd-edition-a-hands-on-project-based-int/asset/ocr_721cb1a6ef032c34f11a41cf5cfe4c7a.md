```
self.settings.screen_height = self.screen.get_rect().height
pygame.display.set_caption("Alien Invasion")
```

When creating the screen surface, we pass a size of `(0, 0)` and the parameter `pygame.FULLSCREEN` ❶. This tells Pygame to figure out a window size that will fill the screen. Because we don’t know the width and height of the screen ahead of time, we update these settings after the screen is created ❷. We use the `width` and `height` attributes of the screen’s rect to update the `settings` object.

If you like how the game looks or behaves in fullscreen mode, keep these settings. If you liked the game better in its own window, you can revert back to the original approach where we set a specific screen size for the game.

> **NOTE**
> 
> *Make sure you can quit by pressing Q before running the game in fullscreen mode; Pygame offers no default way to quit a game while in fullscreen mode.*

# A Quick Recap

In the next section, we’ll add the ability to shoot bullets, which involves adding a new file called *bullet.py* and making some modifications to some of the files we’re already using. Right now, we have three files containing a number of classes and methods. To be clear about how the project is organized, let’s review each of these files before adding more functionality.

# *alien_invasion.py*