# TRY IT YOURSELF

**12-3. Pygame Documentation:** We’re far enough into the game now that you might want to look at some of the Pygame documentation. The Pygame home page is at [https://pygame.org](https://pygame.org), and the home page for the documentation is at [https://pygame.org/docs](https://pygame.org/docs). Just skim the documentation for now. You won’t need it to complete this project, but it will help if you want to modify *Alien Invasion* or make your own game afterward.

**12-4. Rocket:** Make a game that begins with a rocket in the center of the screen. Allow the player to move the rocket up, down, left, or right using the four arrow keys. Make sure the rocket never moves beyond any edge of the screen.

**12-5. Keys:** Make a Pygame file that creates an empty screen. In the event loop, print the `event.key` attribute whenever a `pygame.KEYDOWN` event is detected. Run the program and press various keys to see how Pygame responds.

# Shooting Bullets

Now let’s add the ability to shoot bullets. We’ll write code that fires a bullet, which is represented by a small rectangle, when the player presses the spacebar. Bullets will then travel straight up the screen until they disappear off the top of the screen.

# Adding the Bullet Settings

At the end of the `__init__()` method, we’ll update *settings.py* to include the values we’ll need for a new Bullet class:

```
```python
# settings.py

def __init__(self):
    --snip--
    # Bullet settings
    self.bullet_speed = 2.0
    self.bullet_width = 3
    self.bullet_height = 15
    self.bullet_color = (60, 60, 60)
```
```