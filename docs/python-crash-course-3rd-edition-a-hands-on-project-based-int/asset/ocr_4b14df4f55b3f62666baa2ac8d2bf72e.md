# Game Over!

*Alien Invasion* feels more complete now, but the game never ends. The value of `ships_left` just grows increasingly negative. Let’s add a `game_active` flag, so we can end the game when the player runs out of ships. We’ll set this flag at the end of the `__init__()` method in `AlienInvasion`:

```
**alien_invasion.py**

```python
def __init__(self):
    --snip--
    # Start Alien Invasion in an active state.
    self.game_active = True
```
```

Now we add code to `_ship_hit()` that sets `game_active` to `False` when the player has used up all their ships:

```
**alien_invasion.py**

```python
def _ship_hit(self):
    """Respond to ship being hit by alien."""
    if self.stats.ships_left > 0:
        # Decrement ships left.
        self.stats.ships_left -= 1
        --snip--
        # Pause.
        sleep(0.5)
    else:
        self.game_active = False
```
```

Most of `_ship_hit()` is unchanged. We’ve moved all the existing code into an `if` block, which tests to make sure the player has at least one ship remaining. If so, we create a new fleet, pause, and move on. If the player has no ships left, we set `game_active` to `False`.

# Identifying When Parts of the Game Should Run

We need to identify the parts of the game that should always run and the parts that should run only when the