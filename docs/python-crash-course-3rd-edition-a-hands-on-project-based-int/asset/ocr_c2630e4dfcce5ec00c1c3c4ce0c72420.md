Finally, let’s display the number of ships the player has left, but this time, let’s use a graphic. To do so, we’ll draw ships in the upper-left corner of the screen to represent how many ships are left, just as many classic arcade games do.

First, we need to make Ship inherit from Sprite so we can create a group of ships:

***ship.py***

```
import pygame
from pygame.sprite import Sprite

❶ class Ship(Sprite):
    """A class to manage the ship."""

    def __init__(self, ai_game):
        """Initialize the ship and set its starting position."""
❷      super().__init__()
        --snip--
```

Here we import Sprite, make sure Ship inherits from Sprite ❶, and call super() at the beginning of __init__() ❷.

Next, we need to modify Scoreboard to create a group of ships we can display. Here are the import statements for Scoreboard:

***scoreboard.py***

```
import pygame.font
from pygame.sprite import Group

from ship import Ship
```

Because we’re making a group of ships, we import the Group and Ship classes.

Here’s __init__():

***scoreboard.py***