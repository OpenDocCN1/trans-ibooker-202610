all these values in one place. This approach allows us to work with just one settings object anytime we need to access an individual setting. This also makes it easier to modify the game’s appearance and behavior as our project grows. To modify the game, we’ll change the relevant values in settings.py, which we’ll create next, instead of searching for different settings throughout the project.

Create a new file named settings.py inside your alien_invasion folder, and add this initial Settings class:

**settings.py**

```
```python
class Settings:
    """A class to store all settings for Alien Invasion."""

    def __init__(self):
        """Initialize the game's settings."""
        # Screen settings
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (230, 230, 230)
```
```

To make an instance of Settings in the project and use it to access our settings, we need to modify alien_invasion.py as follows:

**alien_invasion.py**

```
```python
--snip--
import pygame

from settings import Settings

class AlienInvasion:
    """Overall class to manage game assets and behavior."""

    def __init__(self):
        """Initialize the game, and create game resources."""
        pygame.init()
        self.clock = pygame.time.Clock()
        self.settings = Settings()
```
```