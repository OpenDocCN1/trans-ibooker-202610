```
```python
# Start each new ship at the bottom center of the screen.
self.rect.midbottom = self.screen_rect.midbottom

def blitme(self):
    """Draw the ship at its current location."""
    self.screen.blit(self.image, self.rect)
```
```

Pygame is efficient because it lets you treat all game elements like rectangles (*rects*), even if they’re not exactly shaped like rectangles. Treating an element as a rectangle is efficient because rectangles are simple geometric shapes. When Pygame needs to figure out whether two game elements have collided, for example, it can do this more quickly if it treats each object as a rectangle. This approach usually works well enough that no one playing the game will notice that we’re not working with the exact shape of each game element. We’ll treat the ship and the screen as rectangles in this class.

We import the pygame module before defining the class. The `__init__()` method of `Ship` takes two parameters: the `self` reference and a reference to the current instance of the `AlienInvasion` class. This will give `Ship` access to all the game resources defined in `AlienInvasion`. We then assign the screen to an attribute of `Ship`, so we can access it easily in all the methods in this class. We access the screen’s rect attribute using the `get_rect()` method and assign it to `self.screen_rect`. Doing so allows us to place the ship in the correct location on the screen.

To load the image, we call `pygame.image.load()` and give it the location of our ship image. This function returns a surface representing the ship, which we assign to `self.image`. When the image is loaded, we call `get_rect()` to access the ship surface’s rect attribute so we can later use it to place the ship.