
The _prep_msg() method needs a self parameter and the text to be rendered as an image (msg). The call to font.render() turns the text stored in msg into an image, which we then store in self.msg_image ①. The font.render() method also takes a Boolean value to turn antialiasing on or off (antialiasing makes the edges of the text smoother). The remaining arguments are the specified font color and background color. We set antialiasing to True and set the text background to the same color as the button. (If you don’t include a background color, Pygame will try to render the font with a transparent background.)

We center the text image on the button by creating a rect from the image and setting its center attribute to match that of the button ②.

Finally, we create a draw_button() method that we can call to display the button onscreen:

button.py

```
def draw_button(self):
    """Draw blank button and then draw message."""
    self.screen.fill(self.button_color, self.rect)
    self.screen.blit(self.msg_image, self.msg_image_rect)
```

We call screen.fill() to draw the rectangular portion of the button. Then we call screen.blit() to draw the text image to the screen, passing it an image and the rect object associated with the image. This completes the Button class.

## Drawing the Button to the Screen

We’ll use the Button class to create a Play button in AlienInvasion. First, we’ll update the import statements:

alien_invasion.py

```
--snip--
from game_stats import GameStats
```

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

![](img/8f15563c16d2a98c4d3115150679b092_460_0.png)

Figure 14-1: A Play button appears when the game is inactive.

## Starting the Game

To start a new game when the player clicks Play, add the following elif block to the end of _check_events() to monitor mouse events over the button:

alien_invasion.py

```
def _check_events(self):
    """Respond to keypresses and mouse events."""
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            --snip--
        elif event.type == pygame.MOUSEBUTTONDOWN:
            mouse_pos = pygame.mouse.get_pos()
            self._check_play_button(mouse_pos)
```

Pygame detects a `MOUSEBUTTONDOWN` event when the player clicks anywhere on the screen ❶, but we want to restrict our game to respond to mouse clicks only on the Play button. To accomplish this, we use `pygame.mouse.get_pos()`, which returns a tuple containing the mouse cursor’s x- and y-coordinates when the mouse button is clicked ❷. We send these values to the new method `_check_play_button()` ❸.

Here’s `_check_play_button()`, which I chose to place after `_check_events()`:

**alien_invasion.py**

```python
def _check_play_button(self, mouse_pos):
    """Start a new game when the player clicks Play."""
    if self.play_button.rect.collidepoint(mouse_pos):
        self.game_active = True
```
```

We use the rect method `collidepoint()` to check whether the point of the mouse click overlaps the region defined by the Play button’s rect ❶. If so, we set `game_active` to `True`, and the game begins!

At this point, you should be able to start and play a full game. When the game ends, the value of `game_active` should become `False` and the Play button should reappear.

## Resetting the Game

The Play button code we just wrote works the first time the player clicks Play. But it doesn’t work after the first game ends, because the conditions that caused the game to end haven’t been reset.

To reset the game each time the player clicks Play, we need to reset the game statistics, clear out the old aliens and bullets, build a new fleet, and center the ship, as shown here:

**alien_invasion.py**

```
def _check_play_button(self, mouse_pos):
    """Start a new game when the player clicks Play."""
    if self.play_button.rect.collidepoint(mouse_pos):
        # Reset the game statistics.
        self.stats.reset_stats()
        self.game_active = True

        # Get rid of any remaining bullets and aliens.
        self.bullets.empty()
        self.aliens.empty()

        # Create a new fleet and center the ship.
        self._create_fleet()
        self.ship.center_ship()
```

We reset the game statistics ❶, which gives the player three new ships. Then we set `game_active` to `True` so the game will begin as soon as the code in this function finishes running. We empty the aliens and bullets groups ❷, and then we create a new fleet and center the ship ❸.

Now the game will reset properly each time you click Play, allowing you to play it as many times as you want!

## Deactivating the Play Button

One issue with our Play button is that the button region on the screen will continue to respond to clicks even when the Play button isn’t visible. If you click the Play button area by accident after a game begins, the game will restart!

To fix this, set the game to start only when `game_active` is `False`:

**alien_invasion.py**

```
def _check_play_button(self, mouse_pos):
    """Start a new game when the player clicks Play."""
    button_clicked = self.play_button.rect.collidepoint(mouse_pos)
    if button_clicked and not self.game_active:
```

```
```python
# Reset the game statistics.
self.stats.reset_stats()
--snip--
```
```

The `button_clicked` stores a True or False value ❶, and the game will restart only if Play is clicked *and* the game is not currently active ❷. To test this behavior, start a new game and repeatedly click where the Play button should be. If everything works as expected, clicking the Play button area should have no effect on the gameplay.

## Hiding the Mouse Cursor

We want the mouse cursor to be visible when the game is inactive, but once play begins, it just gets in the way. To fix this, we’ll make it invisible when the game becomes active. We can do this at the end of the `if` block in `_check_play_button()`:

```
```python
alien_invasion.py

def _check_play_button(self, mouse_pos):
    """Start a new game when the player clicks Play."""
    button_clicked = self.play_button.rect.collidepoint(mouse_pos)
    if button_clicked and not self.game_active:
        --snip--
        # Hide the mouse cursor.
        pygame.mouse.set_visible(False)
```
```

Passing False to `set_visible()` tells Pygame to hide the cursor when the mouse is over the game window.

We’ll make the cursor reappear once the game ends so the player can click Play again to begin a new game. Here’s the code to do that:

```
```python
alien_invasion.py

def _ship_hit(self):
    """Respond to ship being hit by alien."""
```
```

We’ll first reorganize the `Settings` class to group the game settings into static and dynamic ones. We’ll also make sure any settings that change during the game reset when we start a new game. Here’s the `__init__()` method for *settings.py*:

# settings.py

```
def __init__(self):
    """Initialize the game's static settings."""
    # Screen settings
    self.screen_width = 1200
    self.screen_height = 800
    self.bg_color = (230, 230, 230)

    # Ship settings
    self.ship_limit = 3

    # Bullet settings
    self.bullet_width = 3
    self.bullet_height = 15
    self.bullet_color = 60, 60, 60
    self.bullets_allowed = 3

    # Alien settings
    self.fleet_drop_speed = 10

    # How quickly the game speeds up
    self.speedup_scale = 1.1

    self.initialize_dynamic_settings()
```

We continue to initialize settings that stay constant in the `__init__()` method. We add a `speedup_scale` setting ❶ to control how quickly the game speeds up: a value of 2 will double the game speed every time the player reaches a new level; a value of 1 will keep the speed constant. A value like 1.1 should increase the speed enough to make the game challenging but not impossible. Finally, we call the `initialize_dynamic_settings()` method to initialize the values for attributes that need to change throughout the game ❷.
