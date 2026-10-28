self.y ①. We then use the value of self.y to set the value of self.rect.y ②.

The bullet_speed setting allows us to increase the speed of the bullets as the game progresses or as needed to refine the game’s behavior. Once a bullet is fired, we never change the value of its x-coordinate, so it will travel vertically in a straight line even if the ship moves.

When we want to draw a bullet, we call draw_bullet(). The draw.rect() function fills the part of the screen defined by the bullet’s rect with the color stored in self.color ③.

# Storing Bullets in a Group

Now that we have a Bullet class and the necessary settings defined, we can write code to fire a bullet each time the player presses the spacebar. We’ll create a group in AlienInvasion to store all the active bullets so we can manage the bullets that have already been fired. This group will be an instance of the pygame.sprite.Group class, which behaves like a list with some extra functionality that’s helpful when building games. We’ll use this group to draw bullets to the screen on each pass through the main loop and to update each bullet’s position.

First, we’ll import the new Bullet class:

```
alien_invasion.py

--snip--
from ship import Ship
from bullet import Bullet
```

Next we’ll create the group that holds the bullets in __init__():

```
alien_invasion.py

    def __init__(self):
        --snip--
```