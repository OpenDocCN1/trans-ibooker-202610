```
```python
# Reset the game statistics.
self.stats.reset_stats()
--snip--
```
```

The `button_clicked` stores a True or False value ❶, and the game will restart only if Play is clicked *and* the game is not currently active ❷. To test this behavior, start a new game and repeatedly click where the Play button should be. If everything works as expected, clicking the Play button area should have no effect on the gameplay.

# ## Hiding the Mouse Cursor

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