```
```python
super().__init__()
self.screen = ai_game.screen

# Load the alien image and set its rect attribute.
self.image = pygame.image.load('images/alien.bmp')
self.rect = self.image.get_rect()

# Start each new alien near the top left of the screen.
self.rect.x = self.rect.width
self.rect.y = self.rect.height

# Store the alien's exact horizontal position.
self.x = float(self.rect.x)
```
```

Most of this class is like the `Ship` class, except for the alien’s placement on the screen. We initially place each alien near the top-left corner of the screen; we add a space to the left of it that’s equal to the alien’s width and a space above it equal to its height ①, so it’s easy to see. We’re mainly concerned with the aliens’ horizontal speed, so we’ll track the horizontal position of each alien precisely ②.

This `Alien` class doesn’t need a method for drawing it to the screen; instead, we’ll use a Pygame group method that automatically draws all the elements of a group to the screen.

# Creating an Instance of the Alien

We want to create an instance of `Alien` so we can see the first alien on the screen. Because it’s part of our setup work, we’ll add the code for this instance at the end of the `__init__()` method in `AlienInvasion`. Eventually, we’ll create an entire fleet of aliens, which will be quite a bit of work, so we’ll make a new helper method called `_create_fleet()`.

The order of methods in a class doesn’t matter, as long as there’s some consistency to how they’re placed. I’ll place `_create_fleet()` just before the `_update_screen()` method, but